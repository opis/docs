---
layout: landing-project
title: Opis Http | PHP Library
description: A PHP library that provides an object-oriented representation for http messages.
canonical: /http/
ga: true
project:
    title: Opis Http
    description: HTTP abstraction layer
    docs_url: /http/3.x/
    github: https://github.com/opis/http
    artwork: http
    license: Apache 2.0
---
{% capture about_section %}
Opis Http helps developers to overcome PHP's lack of built-in support for HTTP messages,
by providing a group of classes that offers an object-oriented implementation,
which will result into delivering more robust web applications.
{% endcapture %}
{% include section/lead.html
title="Forget about PHP quirks"
icon=page.project.artwork
content=about_section %}

{% capture about_section %}
Opis Http will help you get rid of all the superglobals
from your code, making it more clean, more testable, and more easy to maintain over long periods of time, especially
when you are building large-scale web applications.
{% endcapture %}
{% include section/description.html
title="No more superglobals"
invert=false
img="terminal"
content=about_section %}

{% include section/modern.html name=page.project.title %}