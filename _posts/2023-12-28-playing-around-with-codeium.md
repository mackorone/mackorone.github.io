---
tags: [software]
---

Last weekend, I decided it was finally time to install an AI coding assistant on
my personal computer.

I initially started looking at GitHub Copilot--the de facto AI coding
assistant--but shied away once I realized it requires a paid subscription. I
already pay for ChatGPT plus, and worst case scenario I should be able to use
*that* subscription for my coding assistant needs. Thankfully, it didn't come to
that.

After a few Google searches, I found [Codeium](https://codeium.com/), which is
free for individuals. The company behind it is, ["committed to
having an industry leading free tier
forever."](https://codeium.com/blog/how-is-codeium-free) I doubt it, but that's
good enough for me for now.

Installation was straightforward, other than the fact that I had to learn Vim
package management first (which I've been meaning to do anyway). The
[Codeium GitHub repo](https://github.com/Exafunction/codeium) had several
suggestions, and [vim-plug](https://github.com/junegunn/vim-plug) caught my eye
for its advertised easy-of-use. So I installed it:
```bash
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

And then added the following to my `~/.vimrc`:
```bash
call plug#begin('~/.vim/plugged')
Plug 'Exafunction/codeium.vim', { 'branch': 'main' }
call plug#end()
```

After which, installing Codeium was as simple as running `:PlugInstall`. Nice!

Once installed, I had to authenticate with Codeium's servers via `:Codeium
Auth`; I assume the access token I copy-pasted into my Vim console was saved
locally somewhere, but I haven't bothered to look for it yet.

Anyway, surprisingly, Codeium seems to... just work? Don't get me wrong, the
completions aren't mind-blowing, but they definitely make writing boilerplate
less tedius. Here are a few examples:

---
**Prompt**:
```bash
# Bash trim suffix from a string
```
**Completion**:
```bash
# Bash trim suffix from a string
trim_suffix() {
  local string="$1"
  local suffix="$2"
  echo "${string%$suffix}"
}
```
**Commentary**: I think this works?

---
**Prompt**:
```python
# Write a Python function to
```
**Completion**:
```python
# Write a Python function to trim suffix from a string
def trim_suffix(string, suffix):
    return string[:-len(suffix)]
```
**Commentary**: Interesting! Codeium was smart enough to catch on to what I was
doing, and complete the comment, along with the code. It looks correct to me,
though it doesn't include type hints and doesn't work if the string doesn't
actually end with the given suffix.

---
**Prompt**:
```markdown
**Prompt**:
```java
```
**Completion**:
````markdown
**Prompt**:
```java
# Write a Java function to
```
**Completion**:
```java
# Write a Java function to trim suffix from a string
public static String trimSuffix(String string, String suffix) {
    return string.substring(0, string.length() - suffix.length());
}
```
````
**Commentary**: Ok, now this is cool. Codeium figured out that I'm currently
writing a Markdown blog post, and predicted the content of the next prompt *and*
it's completion, and provided Markdown content for both. Sure, it gets the Java
comment syntax wrong, but this code is out of context. (I wasn't originally
planning on doing anything meta in the examples section, but this was too cool
not to include.)

---

Maybe I'll write a more detailed review of Codeium once I've had more time
with it. For now, it's a nice, free productivity win.

A few final notes:

1. I had to add this line to my `~/.vimrc` in order for the completions to
render in dark gray.

    ```vim
    set t_Co=16
    ```

1. I changed the default key bindings in my `~/.vimrc`. Now it's `C-y` to
toggle manual mode and `C-o` to cycle through completions. Just works better
for me.

    ```vim
    function! ToggleCodeiumEnabled()
      let g:codeium_manual = !g:codeium_manual
      echo "Codeium autocompletion is " . (g:codeium_manual ? "off" : "on")
    endfunction

    let g:codeium_manual = v:true
    imap <C-y>   <Cmd>call ToggleCodeiumEnabled()<CR>
    imap <C-o>   <Cmd>call codeium#CycleCompletions(1)<CR>
    ```

1. I disabled telemetry. I don't want Codeium training off of my files. God
forbid I accidentally open up a file with sensitive information in it.
Additionally, I configured Codeium to start in "manual" mode. Presumably
this means Codeium won't look at the content until I request it to, but who
knows. (The code is open source, so I could look, but I'm busy writing this
post.)
