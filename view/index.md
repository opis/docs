---
layout: landing-project
title: Opis View | PHP Library
description: An innovative rendering system that can be used with multiple PHP template engines at the same time
canonical: /view/
ga: true
project:
    title: Opis View
    description: Innovative rendering system
    docs_url: /view/5.x/
    github: https://github.com/opis/view
    artwork: view
    license: Apache 2.0
---
{% capture about_section %}
Opis View is an innovating rendering system that takes a new approach when it comes to render HTML templates.
It does not enforce you to use a predefined folder structure or file naming.
Actually, it does not enforce you to use your local filesystem at all.
You can store your templates in RAM, into a database, or even in the cloud!
{% endcapture %}
{% include section/lead.html
title="A fresh perspective"
icon=page.project.artwork
content=about_section %}

{% capture about_section %}
Stop worrying about what template engine to use and shift your focus on other important things.
There's no need to choose! You can use multiple template engines at the same time.
And if you ever decide for a single template engine, you can easily migrate everything on the fly.
It's that simple!
{% endcapture %}
{% include section/description.html
title="Multiple template engines"
invert=false
img="terminal"
content=about_section %}

{% include section/modern.html name=page.project.title %}