---
tags: [software]
---

A while back, I hacked up a solution for automatically activating Python virtual environments in new Tmux panes and windows: [Tmux and Venv]({% post_url 2020-02-23-tmux-and-venv %}).
At the time, I knew about [`direnv`](https://github.com/direnv/direnv), but it wasn't immediately obvious how to get it to work with virtual environments.
There were a few tutorials, but nothing plug-and-play, so I added it to my "I'll get to it eventually" list.

Fast forward to today, and `direnv` now does *exactly* what I want. In particular, after installing it:
```bash
# https://github.com/direnv/direnv/blob/master/docs/installation.md#from-binary-builds
curl -sfL https://direnv.net/install.sh | bash
```

And adding these lines to the bottom of my `.bashrc`:
```bash
# https://github.com/direnv/direnv/wiki/Python#restoring-the-ps1
show_virtual_env() {
    if [[ -n "$VIRTUAL_ENV" && -n "$DIRENV_DIR" ]]; then
        echo "($(basename $VIRTUAL_ENV)) "
    fi
}
export -f show_virtual_env
PS1='$(show_virtual_env)'$PS1

# https://github.com/direnv/direnv/blob/master/docs/hook.md
eval "$(direnv hook bash)"
```

I can create a per-directory `.envrc` file:
```bash
# https://github.com/direnv/direnv/wiki/Python
export VIRTUAL_ENV=venv
layout python python3.10
```

Such that `direnv` automatically creates a virtual environment if necessary, activates it upon entering the directory, and deactivates it upon departure.
With this solution, I can completely get rid of my old hack.

Nice.

---

Side note: after editing an `.envrc` file, you must run the following to confirm that the file is still trusted:
```bash
direnv allow
```

From the [man page](https://direnv.net/man/direnv.1.html#usage):
> This is the security mechanism to avoid loading new files automatically.
> Otherwise any git repo that you pull, or tar archive that you unpack, would be able to wipe your hard drive once you cd into it.

To avoid this step, edit the file via `direnv`:
```bash
direnv edit
```
