---
title: "Jekyll: render markdown in HTML without plugins"
---

*Note: this method is rather hack-ish, but its the cleanest I've seen*

I built this site with Jekyll. Its great because you can create blog posts using the markdown syntax and your content is separate from the theme. However, this magic can only be applied to *documents* from a *collection*. So if you wanted to have markdown in a *page*, and at the same time decouple your HTML from your content, there's no easy way to do so. In most solutions offered by others, you'll have to [use a plugin or have your content in the HTML](http://stackoverflow.com/questions/15917463/embedding-markdown-in-jekyll-html). Yuck!

(Plus, GitHub pages doesn't let you use plugins anymore!)


## Workaround without the use of plugins!

However, I have found a relatively clean workaround using the `include_relative` tag to include pre-rendered HTML outputs from a *collection* made soley for this purpose (it can be resused to do multiple includes). To further illustrate:

**Create a *collection* for all of such embeds.**

Create a *collection* named `include` in *_config.yml. (It could be named anything.)

``` yaml
# collections:
  includes:
    output: false # this method doesn't need an output!
    
# defaults:
  -
    scope:
      path:   "_includes"
      type:   "includes"
    values:
      layout: "include"
```

**Create your markdown content and embed!**

1. Create a *_includes* folder in the root directory if you don't have it already.
2. Drop your markdown file into *_includes* and give it a title. (Lets assume our title is "lorem" for now.)
3. Use this snippet to embed the pre-rendered HTML output.

``` liquid
{{ "{% for include_content in site.includes %}}}
  {{ "{% if include_content.title == "lorem" %}}}
    {{ "{{ include_content.content }}}}
  {{ "{% endif %}}}
{{ "{% endfor %}}}
```

## And there we have it!

If you want to see a full example, head over to the [GitHub repo](https://github.com/liaujianjie/liaujianjie.github.io) for this blog and have a look at the implementation of `.../index.html`.

(Pardon the mess, I've only started using Jekyll today.)