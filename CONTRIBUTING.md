# Setup

Add to `~/.bashrc`:
```bash
# Install Ruby Gems to ~/gems
export GEM_HOME="$HOME/gems"
export PATH="$HOME/gems/bin:$PATH"
```

Install Jekyll and Bundler:
```bash
$ gem install jekyll bundler
```

Install other gems:
```bash
$ bundle install
```

Start the server:
```bash
$ bundle exec jekyll serve
```
