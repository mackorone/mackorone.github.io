---
---

Thanks to the Steam holiday sale, I recently discovered a puzzle game called
[Snakebird](https://store.steampowered.com/app/357300/Snakebird/). The game
features colorful segmented creatures that traverse platforms to collect fruit.
Similar to the game Snake, Snakebirds crawl by moving their head, with each body
segment moving into the next segment's previous location. For example, here's
how a Snakebird moves up and to the right:

```
                   ^                       ^       +---+           ^       +---+---+ 
 Key:            1 |                     1 |       | H |         1 |       | B | H | 
 H - head          +---+---+---+           +   +---+---+           +       +---+---+ 
 B - body        0 | T | B | H |         0 |   | T | B |         0 |       | T |     
 T - tail          +---+---+---+--->       +---+---+---+--->       +---+---+---+---> 
                     0   1   2   3           0   1   2   3           0   1   2   3
```

Unlike Snake, Snakebird is a platformer; the creatures fall downward until they
contact a solid surface. The combination of platforming and Snake-like movement
makes for some surprisingly challenging puzzles. Beyond the first few levels,
which introduce the core mechanics, each is a gauntlet of trial-and-error. In
order to succeed, you need to constantly think about where each Snakebird needs
to go, and more importantly, how it should get there - its path determines the
shape of its body.

After struggling for *hours* on some of the later levels, I stepped back and
took stock of my options. I was tempted to simply look up the solutions, but
I couldn't bring myself to cheat. Instead, I decided that if I could write a
computer program to brute-force solve the problems - which was basically my
approach at that point anyway - then I technically still solved them. Might as
well put my programming skills to good use.

However, like any good engineer, I spent some time looking for prior art. Lo and
behold, some pretty good solvers already exist:

{% include github_repo_card.html repo="jsnell/snakebird-solver" %}
{% include github_repo_card.html repo="apocalyptech/snakebirdsolver" %}
{% include github_repo_card.html repo="thquinn/Birdsnake" %}

After some introspection, I eventually decided that no, I wasn't going to
implement my own Snakebird solver. No sense in reinventing the wheel. Instead,
if at all, I should contribute to an existing solver - it's the more responsible
choice with respect to advancing society and making the best use of my own
time. Besides, building on others' work is a good habit to practice. I haven't
yet contributed but perhaps one day I will.

Lastly, while searching for solvers, I found a couple of really great Snakebird
clones, each of which is playable in the browser and supports custom levels:

{% include github_repo_card.html repo="M-FF-M/snakebird" %}
{% include github_repo_card.html repo="thejoshwolfe/snakefall" %}
{% include github_repo_card.html repo="Terzalo/Avis-Anguis" %}

Kudos to the community for all of these awesome projects.
