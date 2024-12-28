---
layout: landing-project
title: Opis Closure | PHP Library
description: A PHP library that makes closure serialization possible
canonical: /closure/
ga: true
project:
    title: Opis Closure
    description: Serialize closures, serialize anything
    docs_url: /closure/4.x/
    github: https://github.com/opis/closure
    artwork: closure
    license: MIT
---
{% capture about_section %}
Opis Closure helps you overcome PHP's limitations regarding data serialization,
by providing replacements for PHP's serialize/unserialize methods.
{% endcapture %}
{% include section/lead.html
title="Keep calm and serialize closures"
icon=page.project.artwork
content=about_section %}

{% capture about_section %}
The library is capable of serializing any data - including closures, and also provides helpers to implement 
serialization for exotic structures.
{% endcapture %}
{% include section/description.html
title="Powerful serializer"
invert=false
img="terminal"
content=about_section %}

{% capture about_section %}
Opis Closure is shipped with a fast heuristic parser which supports the latest PHP 8 syntax and automatically
resolves all constants, magic constants, classes, and functions used by a closure.
{% endcapture %}
{% include section/description.html
title="PHP 8.x compatible"
invert=true
img="php8"
content=about_section %}