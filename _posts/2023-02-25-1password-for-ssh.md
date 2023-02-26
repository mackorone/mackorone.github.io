---
title: "Using 1Password for SSH Keys"
tags: [software]
---

1Password [announced](https://blog.1password.com/1password-ssh-agent/) support
for SSH keys in March of 2022. It's been almost a year since then and I finally
got around to setting it up. I wish I'd done so sooner, because I discovered
that, up until then, my SSH keys were sitting on my hard drive unencrypted,
which is apparently a huge no-no if you prefer not to be impersonated. Whoops.

Anyways, setup was actually as easy as 1Password claims:

1. Install 1Password 8 desktop app
1. Enable the 1Password SSH agent
1. Add two lines to `~/.ssh/config`
1. Use 1Password to generate a new SSH key (for GitHub)

There were a few gotchas, however:

1. You *must* start 1Password 8 before using SSH keys (obvious in retrospect)
1. You [*must*](https://developer.1password.com/docs/ssh/agent/#eligible-keys) put your SSH keys in your personal vault ([for now](https://1password.community/discussion/128446/feature-request-make-the-ssh-agent-work-with-any-vault))
1. And on Linux, you
[*must*](https://developer.1password.com/docs/ssh/agent/#requirements) use system authentication

Each of those took a while to figure out, especially since they all resulted in
the same error message:
```
git@github.com: Permission denied (publickey).
```

Thankfully, 1Password customer support is fantastic, and I was able to unblock myself by
perusing their community forums
([example](<https://1password.community/discussion/130082/ssh-agent-stopped-working>)).
This command was especially helpful for debugging (`-v` for "verbose"):
```
ssh -vT git@github.com
```

I suppose that I could've avoided the trouble by simply reading more carefully
before beginning, but where's the fun in that?

Lastly, for extra credit, I also enabled
[Git commit signing](https://developer.1password.com/docs/ssh/git-commit-signing).
After using 1Password 8 to automagically update my `~/.gitconfig` and adding my
"Signing Key" to GitHub, I observed my commits show as "Verified" in the GitHub
UI - no subscription necessary!

Once everything was working correctly and I was comfortable with the workflow, I
removed my old, unencrypted SSH keys from GitHub and my hard drive. The end.
