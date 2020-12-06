---
layout: landing-project
title: Opis Container | PHP Library
description: An easy to use and fully serializable dependency injection container that can be used with any PHP framework
canonical: /container/
ga: true
project:
    title: Opis Container
    description: Dependency injection container
    docs_url: /container/3.x/
    github: https://github.com/opis/container
    artwork: container
    license: Apache 2.0
---
{% capture about_section %}
Opis Container provides a lightweight, fully serializable, dependency injection container, that
can be used in both small and enterprise-scale web applications.
{% endcapture %}
{% include section/lead.html
title="Serializable container"
icon=page.project.artwork
content=about_section %}

{% capture about_section %}
The container provided by the Opis Container library is fully compatible with the PSR-11 specification,
allowing you to use it along with other libraries that have support for it.
{% endcapture %}
{% include section/description.html
title="PSR-11 compatible"
invert=false
img="terminal"
content=about_section %}

{% include section/modern.html name=page.project.title %}