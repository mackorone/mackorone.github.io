---
tags: [software]
---

Sometimes it's tough to find the energy to write a blog post. I have a long list
topics that I want to write about, but it's hard to find the willpower to fire
up a text editor and start riffing. To make things worse, Jekyll blog post file
names have the following format: `yyyy-mm-dd-title.md`. As such, it can be
annoying to:

1. Open up a post - I'm not good at typing numbers
2. Update the date - sometimes it takes multiple days to finish a post

So I built a tool that reduces the friction involved in managing Jekyll blog
posts: [`mackorone/jekyll-blog-cli`](https://github.com/mackorone/jekyll-blog-cli)

(Yes, I was procrastinating...)

Key features include:
- Open up posts and update dates *by title*, rather than path
- Tab completion on title using the
  [`argcomplete`](https://pypi.org/project/argcomplete/) library

In short, I never have to manually type date strings - nice.

#### Demo

List blog posts by date:
```
$ blog list
2019-07-14-hello-world.md
2019-07-17-running-upright-relieves-cramps.md
...
```

Open a post by title (with tab-completion):
```
$ blog open <TAB>
hello-world.md
honda-maintenance-codes.md
...
$ blog open hello-world.md
```

Update the date by title:
```
$ blog touch hello-world.md
```
```
Before: 2019-07-14-hello-world.md
After: 2019-11-19-hello-world.md
```

Run Jekyll server locally:
```
$ blog serve
```
