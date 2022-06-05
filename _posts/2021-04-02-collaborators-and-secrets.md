---
tags: [software]
---

Last week, I migrated CI for my open source projects from Travis CI to GitHub
Actions. While I was configuring repository secrets, some text at the top of
the page caught my attention:

>  Secrets are environment variables that are encrypted. Anyone with
>  collaborator access to this repository can use these secrets for Actions. 

It took me a moment, but I realized that if contributors can use my secrets for
Actions, then they can read those secrets. They only need to modify an action to
capture passed secrets and send them to some external server. And they can do it
in a single commit, before I have a chance to intervene.

Following that realization, I quickly revoked all contributor permissions to my
secret-bearing repositories and sent out the following email:

> I revoked your contributor permissions to [repo] to avoid accidentally
> leaking repository secrets. I just realized that, if you had been a malicious
> actor, you could have stolen the secrets by pushing secret-stealing code
> upstream. Obviously I know you're not a malicious actor, but I decided to
> revoke access simply as a matter of best practice.

Thankfully, everyone was understanding.

The interesting part about this story is that I was completely blind to this
attack vector until I migrated from Travis CI to GitHub Actions. The fact that
the my secrets and source code lived in two different places made it difficult
to see the connection between the two. Proximity matters. It's no coincidence
that security exploits often consist of many steps, each of which needs to be 
innocuous enough to avoid scruple.

And there's probably something to be said about "worst case" messaging when it comes
to security. Rather than talking about each hop independently, tools
should warn users of downstream effects directly. There's a nontrivial
difference between, "Collaborators can use these secrets for Actions," and,
"Collaborators can read your secrets." Why force each user to reason about
the ramifications on their own? Sure, you'd need to draw a line between useful
and overkill, and the transitive access problem isn't generally
solvable, but better messaging feels like an easy win.
