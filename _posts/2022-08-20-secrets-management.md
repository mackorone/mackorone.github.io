---
tags: [software]
---

#### Introduction

For the longest time, I stored secrets (API keys, passwords, etc.) in a
persistent, unencrypted, plain text file on my local hard drive. My `~/.bashrc`
would read the file and export the secrets as environment variables. Recently,
the lack of security started to bother me so I built something better.
My requirements were as follows:

1. Don't persist unencrypted secrets on the filesystem indefinitely
1. Only require a decryption password once per session
1. Continue to export secrets as environment variables
1. Gracefully handle naming collisions between projects

#### Research and Design

After a bit of research, I discovered the
[1Password CLI](https://developer.1password.com/docs/cli) and realized that it 
solved the most important problem: where to persist the secrets. I already use
1Password for all of my sensitive information so it makes sense to store my
developer secrets there, too. The CLI makes it easy to fetch the secrets when I
need them, rather than keep them on my hard drive, solving requirement #1.

To solve requirement #2, I decided to just write all secrets fetched via the
1Password CLI to a temporary file, `/tmp/my_secrets.sh`. Once fetched, I can
read secrets from that file, rather than invoking the 1Password CLI again. Files
in `/tmp` are removed on reboot, which is good enough for me. In the future, I
may create a dedicated [tmpfs](https://en.wikipedia.org/wiki/Tmpfs) for the
file.

To solve requirement #3, I decided to read from `/tmp/my_secrets.sh` within my
`~/.bashrc` and export the values as environment variables - same as before,
except now the file lives in `/tmp`. This ensures that any new Tmux panes and
windows will have the environment variables defined.

To solve requirement #4, I decided to give my environment variables globally
unique names and coerce them to more generic names using `direnv`.

#### Implementation
Here's what I've got in my `~/.bashrc` (see [`secrets.sh`](https://github.com/mackorone/dotfiles/blob/main/bashrc/secrets.sh)):
```bash
SECRETS_FILE='/tmp/my_secrets.sh'

if [[ -r "$SECRETS_FILE" ]]; then
    # shellcheck source=/dev/null
    source "$SECRETS_FILE"
fi

secrets() {
    # If no arguments provided, remove the file
    if [[ -z "$*" ]]; then
        rm "$SECRETS_FILE"
        return $?
    fi

    # Ensure all arguments are readable files
    for arg in "$@"
    do
        if [[ ! -r "$arg" ]]; then
            echo "Unable to read '$arg'"
            return 1
        fi
    done

    # Concatenate, inject, and write file
    ( 
        TOKEN="$(op signin --raw)" && \
        cat "$@" | op inject --session "$TOKEN" > "$SECRETS_FILE"
    ) 
}
```

The `secrets` function behaves as follows:
1. If no arguments are provided, it removes `$SECRETS_FILE` - a
convenience for quickly getting rid of plain text secrets on my filesystem
1. Otherwise, if one more file paths are provided, it concatenates their
contents, injects secrets, and writes the output to `$SECRETS_FILE`

By "injects secrets" I mean replaces references to secrets with the secrets
themselves. For example, suppose I had the following two files:
```bash
# foo.sh
export MY_FOO_PASSWORD='op://secrets/foo/password'
```

```bash
# bar.sh
export MY_BAR_PASSWORD='op://secrets/bar/password'
```

Invoking the `secrets` function as follows:
```bash
secrets foo.sh bar.sh
```

Would result in the following getting written to `$SECRETS_FILE`:
```bash
export MY_FOO_PASSWORD='abc123'
export MY_BAR_PASSWORD='def456'
```

After which, I can run `source ~/.bashrc` to make the environment variables
available to the current shell. All new shells will get them for free - no need
to manually `source`.

The concatenation part is nice because it keeps the system simple and allows
me to fetch exactly the secrets I need and nothing more. Also, note that the
contents of `foo.sh` and `bar.sh` are arbitrary. I've defined them to export
environment variables, but they can do anything. I have a private GitHub repo
that contains all of those template files - private because I don't want to
reveal the existence of other private projects by publishing references to
their secrets.

Lastly, within each of my projects, I have an `.envrc` file that looks like:
```bash
# foo/.envrc
export PASSWORD="$MY_FOO_PASSWORD"
```

That way, within the source code, I can just use `$PASSWORD` without the extra
qualifiers.

#### Shell Prompt

I've also got the following in my `~/.bashrc`:

```bash
SECRETS_PREFIX='MY_'

show_secrets_are_present() {
    local output=''
    if [[ -r "$SECRETS_FILE" ]]; then
        output="$output🔏"
    fi
    if (printenv | grep "$SECRETS_PREFIX" &> /dev/null); then
        output="$output🔓"
    fi
    if [[ -n "$output" ]]; then
        echo -n "$output "
    fi
}

PS1='$(show_secrets_are_present)'$PS1
```

Which ensures that I always know when secrets are available in `$SECRETS_FILE`
or environment variables within the current shell - a reminder to be mindful:
```bash
🔏 >  # Secrets are on the filesystem
🔓 >  # Secrets are in the environment
🔏🔓 >  # Both
```

#### Notes

- I briefly researched 1Password [Secrets Automation](https://developer.1password.com/docs/connect/)
but got turned off by the idea of spending more money and/or running a Docker
container locally. (Maybe I misunderstood? Someone correct me.)

- I played around with loading secrets via `direnv`, but doing so requires 
a 1Password CLI session and I don't like the idea of being logged into
1Password on the command line indefinitely.

- I also played around with injecting secrets via `op run`, but it requires
a 1Password CLI session (bad) or password re-entry on every invocation, neither
of which I found appealing.

- I use a separate vault, "Secrets," for all of my developer secrets. It just
helps me to stay organized. Ideally, I could configure 1Password to only allow
passwords in that vault to be read by the CLI, but alas,
[that's not possible](https://1password.community/discussion/122006/restrict-access-for-command-line-tool-feature-request). Notably, this means that all of my sensitive information
is accessible via CLI - prior to this project, it wasn't - but that's okay
because it's basically the same exposure as the 1Password browser plugins.

- Lastly, some related links, i.e., inspiration for this project:
  - <https://github.com/direnv/direnv/issues/597>
  - <https://1password.community/discussion/128381/>
