---
title: "1Password Service Accounts"
tags: [software]
---

A few months ago, I ran into an interesting problem with my
[secrets management]({% post_url 2022-08-20-secrets-management %})
setup: I needed to access a one-time password (OTP) in some piece of software,
but my setup was only designed to resolve secrets references once per session.

The solution turned out to be surprisingly simple: create a
[service account](https://developer.1password.com/docs/service-accounts/),
generate a token, and retrieve that token once per session. Then, use the
token to fetch other secrets at runtime. There's even an
`OP_SERVICE_ACCOUNT_TOKEN` environment variable for the 1Password CLI that makes
it trivial to persist the token and use it for subsequent CLI commands.

Given a template file:
```bash
# my_template.sh
export OP_SERVICE_ACCOUNT_TOKEN='op://path/to/token'
```
My [script](https://github.com/mackorone/dotfiles/blob/main/bashrc/secrets.sh)
basically runs the following:
```bash
eval $(op inject --session "$(op signin --raw)" --in-file my_template.sh)
```

After which, CLI commands like these will just work:
```bash
op read op://vault/item/name
op item get --vault vault --otp name
```

Persisting the token in the environment made me nervous at first--it's
essentially a key for accessing *all* of my passwords. But it turns out you can
configure service accounts to only have access to specific vaults, so I created
a dedicated vault containing *only* the secrets needed by the piece of software.
Thus, as before, only the relevant secrets are accessible within the session.

In conclusion, as the saying goes, "All problems in computer science can be solved
by another level of indirection."
