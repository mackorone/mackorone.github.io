---
tags: [software]
---

I spent the better part of this morning learning about Python package
management:
- By default, Python packages are installed by name only, which means it's
  impossible to install multiple versions of the same package
- `virtualenv` addresses that issue by creating a separate directory for each
  "virtual environment," each of which has its own package versions
- Each virtual environment has a script called `activate` that modifies `PATH`
  to intercept Python-like commands (e.g., `python`, `pip`), to ensure that the
  proper versions are used
- `venv` is the officially supported successor to `virtualenv`
    - `venv` used to be called `pyvenv`, which is confusingly similar to
      `pyenv`, a tool that makes it easy to manage multiple Python versions
    - Each `venv` virtual environment has its own Python binary which matches the
      version that was used to create the environment
      ([link](https://docs.python.org/3/library/venv.html))
    - To use Python3.8 within the virtual environment, you need to create the
      virtual environment like this: `python3.8 -m venv my_venv`
- There are multiple versions of the `venv` - one per Python version. You need to
  have the proper version installed before you can create a virtual environment
  for a particular Python version: `sudo apt-get install python3.8-venv`
- In general, avoid touching the system `pip`. Instead, create
  virtual environments, and install/upgrade `pip` and other packages within
  them.

Some useful links:
- [Python Virtual Environments: A Primer](https://realpython.com/python-virtual-environments-a-primer/)
- [Managing Multiple Python Versions With pyenv](https://realpython.com/intro-to-pyenv/)
- [*"What is the difference between venv, pyvenv, pyenv, virtualenv,
  virtualenvwrapper, pipenv, etc?"*](https://stackoverflow.com/questions/41573587/what-is-the-difference-between-venv-pyvenv-pyenv-virtualenv-virtualenvwrappe)

After a few hours of research, here are the commands I ran to configure a new project:
```bash
# Install Python3.8 and venv - one time thing
sudo apt-get install python3.8 python3.8-venv

# Initialize a new project folder
mkdir ~/project
cd ~/project
git init

# Call the venv module to create a new virtual
# environment in a subdirectory called 'venv'
python3.8 -m venv venv

# Don't track the virtual environment
echo venv >> .gitignore
```

With everything set up, installing dependencies is as easy as:
```
source venv/bin/activate
pip install ...
```

And export dependencies is just:
```
pip freeze > requirements.txt
```

One last thing: I noticed that new `tmux` panes and windows don't inherit the
virtual environment from the parent shell.  Since I frequently create new panes
and windows, it quickly became inconvenient to `activate` the virtual
environment every time I did. So I added the following lines to my `.bashrc`:
```
# Activate virtual env and save the path as a tmux variable,
# so that new panes/windows can re-activate as necessary
function sv() {
    source venv/bin/activate &&
    tmux set-environment VIRTUAL_ENV $VIRTUAL_ENV
}
if [ -n "$VIRTUAL_ENV" ]; then
    source $VIRTUAL_ENV/bin/activate;
fi
```

Assuming I always name my project-specific virtual environments "venv" and use
`sv` to activate them, `tmux` will pass the virtual environment path to new
panes and windows, source my `bashrc`, and automatically `activate` - nice!

Worth mentioning: there are other solutions to the activation problem. For
example, [direnv](https://direnv.net/) lets you configure environment
variables on a per-directory basis. You could use it to run `activate` whenever
you change into project directory, and `deactivate` whenever you leave. For now
though, the lines above are good enough for me.


#### Update ({{ "2021-01-25" | date_to_string }})

With some fiddling, I figured out how to unset the `VIRTUAL_ENV` variable
when `deactivate` is run:

```
function sv() {
    source venv/bin/activate &&
    tmux set-environment VIRTUAL_ENV $VIRTUAL_ENV &&
    alias deactivate='\deactivate && tmux set-environment -u VIRTUAL_ENV && unalias deactivate'
}
```
