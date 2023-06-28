---
layout: post
title:  "Hello World!"
date:   2023-06-28 18:18:24 +0200
tags: update
comments: true
---
A few weeks back I decided I wanted to go deeper into understanding of technology.
Usually my process when starting something new is a bit like this:

1. Get a broad picture of the topic and figure out if it is really worth a second look
2. Read deeper into the subject matter and get an understanding of the fundamentals
3. Setup something that applies this knowledge in a practical way
4. Iterate pratical and theoretical work until I am satisfied

And I thought it might be nice if I could do that at least for some of topics
in a bit more public forum.

## Broad Picture

So, what are the options of starting a blog? Looking through the web, I stumbled
upon multiple options:

- Write something on medium or twitter
- Use a blogging platform like wordpress
- Build something from scratch
- Use github pages to host some markdown files

The latter seemed like an approachable option, which I had though about in the past,
but never found the time or the motivation to do it.

This is, when I found [this post][karpathy] by Andrej Karpathy, which describes
how he switched to jekyll and github pages, which provided me with a starting point.

## Fundamental Understanding

So the next part was to get an understanding of how jekyll actually works.
Jekyll by itself is a static site generator, which means that it takes input
files (e.g. markdown files in a github repo) and converts them into static html.

> This is opposed to a dynamic site, that might feature a backend and more complex
> client-server communication.

This does not mean that all content has to be static, since injection of client side
javascript is still possible (in fact this is how the comment section in this blog works).
But it basically means that all the content is generated at build time and not updated
from a server (like a news site, which might generate popups for breaking events).

## Practical Setup

This part required to jump through a few hoops. Jekyll is written in ruby and while
the mac I use comes with a pre-installed version, this one is pretty outdated.

Installing a new version is not as straight forward as I would have hoped, but
requires to set a bunch of path variables (might be different on other systems):

```bash
brew install ruby@3

# Add to ~/.zshrc
# TODO: add this
```

After that, I could install jekyll and run the local server:

```bash
# assuming you are already in the blog repo
gem install bundler jekyll
jekyll new .
bundle exec jekyll serve
```

This will bring you


## Future Work

Since I also recently started playing around with flutter, I might want to rebuild
this blog (should I stick with it) at some point as a more dynamic webpage.

The benefits to me would be more control over the behavior of the page and injection
of dynamic content (e.g. it would be great to have a search function or inject citations
in a more dynamic fashion).
[This post][flutter-hugo] seems to be a good starting point.

But probably the more immediate goal would be to figure out if I actually find the
time to fill this blog with content (and improving my writting).

As a random note, also a citation that I have found interesting [1] (this is
mainly to set a precedent for how I want to cite papers in the future).

## Citations

[1] S. Pan, L. Luo, Y. Wang, C. Chen, J. Wang, and X. Wu, “Unifying large language models and Knowledge Graphs: A roadmap,” arXiv [cs.CL], Jun. 14, 2023. Accessed: Jun. 26, 2023. [Online]. Available: <http://arxiv.org/abs/2306.08302>

[flutter-hugo]: https://medium.com/funwithflutter/making-a-blog-with-flutter-and-hugo-6da036a346c9
[karpathy]: http://karpathy.github.io/2014/07/01/switching-to-jekyll/