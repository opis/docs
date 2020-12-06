---
layout: landing-project
title: Opis String | PHP Library
description: A PHP library that allows you to work with multibyte strings in an object-oriented manner
canonical: /string/
ga: true
project:
    title: Opis String
    description: Multibyte strings as objects
    docs_url: /string/1.x/
    github: https://github.com/opis/string
    artwork: string
    license: Apache 2.0
---
{% capture about_section %}
Opis String is a tiny library that allows you to work with
multibyte encoded strings in an object-oriented manner. The library has no dependencies to <i>mb_string</i> or
similar PHP extensions.
{% endcapture %}
{% include section/lead.html
title="No more procedural programming"
icon=page.project.artwork
content=about_section %}

{% capture about_section %}
The best thing about treating strings as objects is that you don't have to remember a quintillion function names
and the order of their arguments. You will love using your IDE with this kind of strings!
{% endcapture %}
{% include section/description.html
title="Just like in Java"
invert=false
img="terminal"
content=about_section %}

{% include section/modern.html name=page.project.title %}