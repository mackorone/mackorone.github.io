---
title: "Secrets Management: Minor Improvement"
tags: [software]
---

I recently discovered that the 1Password CLI now integrates nicely with the
1Password desktop app. Similar to
[how it works with SSH]({% post_url 2023-02-25-1password-for-ssh %}),
commands like `op inject` can talk to the app to authenticate, rather than
requiring the user to provide credentials on the command line. This is nice
for the following reasons, in order of importance (descending):

1. The workflow and security model for all of my use cases--filling in passwords
in the browser, SSH requests, signing commits, and ad hoc CLI requests--are now
exactly the same. The 1Password app is the source of truth for if a session is
active anywhere on my machine. When I lock the app, I am certain that nothing
can access any of my passwords without authentication.

1. After the first unlock, which requires the master password, subsequent
requests can use system authentication, which is often more convenient (i.e., a
fingerprint scan)

1. It simplifies my code a little bit:
[`2a57989b`](https://github.com/mackorone/dotfiles/commit/2a57989b0a0f0b7d914b2082413abc8ca87be3f4)

1Password continues to ship useful features. I remain a happy customer.
