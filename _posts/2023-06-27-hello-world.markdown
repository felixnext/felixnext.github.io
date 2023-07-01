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

# Add ruby to the path (replaces system default version)
echo 'export PATH="/opt/homebrew/opt/ruby/bin:$PATH"' >> ~/.zshrc

# to make sure jekyll is on the path add gems folder
# you can validate this path using `gem environment` (the EXECUTABLE DIRECTORY)
echo 'export PATH="/opt/homebrew/lib/ruby/gems/3.2.0/bin:$PATH"' >> ~/.zshrc
```

> Note that this relates to installation on MacOS (related to homebrew and zsh).
> There are also ways to achieve something similar with `rbenv` or `chruby`,
> but using `brew` seemed like the easiest option to me.

After that, I could install jekyll and run the local server:

```bash
# assuming you are already in the blog repo
gem install bundler jekyll
jekyll new .
bundle exec jekyll serve
```

If you know open up you browser (`localhost:4000`) you should see something like
this:

![jekyll default page](/assets/imgs/hello-world/jekyll.png)

Before moving on it is worth taking some time to understand the folder structure.
It should look like this:

![jekyll folder structure](/assets/imgs/hello-world/jekyll-folder.png)

In general folders prefixed with a `_` are used by the system and other folders
that you might create (such as `assets`) are exposed as static content.

Jekyll uses `html` templates to generate pages. You can find these under `_layouts`.
You can access either `page` specific (defined in the front-matter, more on that later)
or `site` specific variables (defined in `_config.yml`) through the use of
`{% raw %}{{ VAR }}{% endraw %}` in the templates.

A simple template might look like this:

```html
{% raw %}
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
  </head>
  <body>
    <h1>{{ page.title }}</h1>
    <p>{{ page.content }}</p>
  </body>
</html>
{% endraw %}
```

> Note that you can also include client-site javascripts as well as css files
> here.

If you would want to write a post, you can do so through a [markdown][markdown]
file, which might look like this:

```markdown
{% raw %}
---
layout: post
title:  "Hello World!"
date:   2023-06-28 18:18:24 +0200
---

Your Markdown Text here
{% endraw %}
```

Remember when I talked about front-matter? That is exactly the `---` section at
the beginning of the file. It contains variables defined for the page, which
can be picked up by the template.

So now that we understand the basics, we can commit the current stage in our git-repo.
The final thing to do is to connect the github pages to my domain.

For that go to your github repository site and navigate to the settings tab.
Under the section `Code and Automations` you should find a blade called `Pages`.
In there you can enable the github-pages functionality and even connect a custom
domain.

To enable the custom domain you will also need to integrate it into the DNS settings
of your domain provider. I will pick the example of a subdomain here (namely `blog.felixnext.xyz`)
that I have currently setup.
To enable, simply create a new CNAME record:

```
blog.felixnext.xyz. 3600 IN CNAME felixnext.github.io.
```

> Note that you might have to wait a bit before github will confirm the correct
> domain settings, as DNS records take a bit to propagate.

After that you should be good to go.

## Iterations

Now that we have a working base-setup we can start the process of iteration,
going into certain aspects in more detail.

### 1. Adding a Theme

You might be a person that is way more design minded than me and want to configure
all html and css templates yourself. If you are not (like me) then you should
consider using a theme and start you customization from there.

I found that the easiest way to set this up is to start from scratch. That means
delete everything that you have so far and first select a base theme that you want
to use (a good starting point browsing for themes is [here][jekyll-themes]).

For this blog I decided to go with [Lanyon][lanyon], which is a theme that is
based on [Poole][poole] theme. To install we would need to follow to simple steps:

1. Download the [Poole][poole] repository and use it as a starting point
2. Download the [Lanyon][lanyon] repository and copy the files into the Poole
   repository

Then start the local server (with `bundle exec jekyll serve`) and verify that
everything is working correctly.

Now you can start the process of customization. For that you would usually start
in the `_config.yml` file that defines site-wide variables like the name of your
blog and a general description.

After that you can dive into the `_layouts` folder and start customizing the
html templates to your liking.

### 2. Add Disqus Comments

Why post if you can't get feedback on your writing? So it might be a great thing
to somehow embed a comment functionality into the blog.

There are multiple options to enable this (like [staticman][staticman] or [utterances][utterances]),
but I ended up using [Disqus][disqus] as it seemed to be the easiest to setup and use.

To get started, sign up on the [Disqus][disqus] website and create a new site.
Here you should provide a unique name for your site.

After that you can already embed the comment functionality in your html templates
(I added it at the bottom of my `post.html` template):

```html
{% raw %}
{% if page.comments %}
<div id="disqus_thread"></div>
<script>
    /**
    *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
    *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables    */
    var disqus_config = function () {
        this.page.url = "https://<YOUR-URL>";
        this.page.identifier = "{{ page.url }}";
    };
    (function() { // DON'T EDIT BELOW THIS LINE
    var d = document, s = d.createElement('script');
    s.src = 'https://<DISQUS-UNIQUE-NAME>.disqus.com/embed.js';
    s.setAttribute('data-timestamp', +new Date());
    (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
{% endif %}
{% endraw %}
```

> Note that you will want to replace the `<YOUR-URL>` and `<DISQUS-UNIQUE-NAME>`
> parts of the code above with your specific values.

After that the system should render without probelms and you can control the comments
widget through the use of the `comments` variable in the front-matter of the post.

### 3. Creating Posts

The final step of our journey into jekyll is to actually write posts for the blog.
I would assume that you are already familiar with [markdown][markdown], but there
are a few details I want to highlight.

{% raw %}
The first thing is the use of variables. Since your markdown files are also parsed
by the site-renderer of jekyll you can use the same `{{ VAR }}`
syntax to access variables as in the html templates. Should you want to avoid this,
you can use the `{ % raw % }` and `{ % endraw % }` tags to escape the parsing
(**Note** remove the space between the brackets and the percent sign).
{% endraw %}

The second thing is the use of images. As custom in markdown you can embed images
with `![ALT TEXT](url)`. To host images on your site, you can use the `assets`
folder. (I would recommend creating an `imgs` subfolder and then a subfolder
for each post).
The final result might look something like:

`![ALT TEXT](/assets/imgs/post-name/image.png)`

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
[markdown]: https://www.markdownguide.org/basic-syntax/
[jekyll-themes]: https://jekyllthemes.io/
[lanyon]: https://github.com/poole/lanyon
[poole]: https://github.com/poole/poole
[staticman]: https://staticman.net/
[utterances]: https://utteranc.es/
[disqus]: https://disqus.com/
