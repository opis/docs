---
layout: landing-project
title: Opis Closure | PHP Library
description: A PHP library that makes closure serialization possible
canonical: /closure/
ga: true
project:
title: Opis Closure
description: Serialize closures (anonymous functions)
docs_url: /closure/3.x/
github: https://github.com/opis/closure
artwork: closure
license: MIT
---
{% capture about_section %}
Opis Closure helps you overcome PHP's limitations regarding closure serialization,
by providing a wrapper that will make them serializable.
{% endcapture %}
{% include section/lead.html
title="Keep calm and serialize closures"
icon=page.project.artwork
content=about_section %}

{% capture about_section %}
The library is capable of serializing not only the closure itself, but also the data structures related to that
particular closure, allowing for a very accurate reconstruction of the closure.
{% endcapture %}
{% include section/description.html
title="Powerful serializer"
invert=false
img="terminal"
content=about_section %}

{% capture about_section %}
Opis Closure is shipped with a fast heuristic parser which supports the latest PHP 7 syntax and automatically
resolves all constants, magic constants, classes, and functions used by a closure.
{% endcapture %}
{% include section/description.html
title="PHP 7.x compatible"
invert=true
img="php"
content=about_section %}