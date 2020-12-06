---
layout: landing-project
title: Opis ORM | PHP Library
description: A powerful new ORM library for PHP that is focused on productivity and developer experience.
canonical: /orm/
ga: true
project:
    title: Opis ORM
    description: Simple, productive and easy to learn
    docs_url: /orm/1.x/
    github: https://github.com/opis/orm
    artwork: orm
    license: Apache 2.0
---
{% capture about_section %}
Opis ORM is a library that provides an implementation for the Data Mapper pattern,
built with the following goals in mind: simplicity, productivity, and a smooth learning curve for the developers.
{% endcapture %}
{% include section/lead.html
title="Powerful ORM library"
icon=page.project.artwork
content=about_section %}

{% capture about_section %}
We wanted to create something that is easy to grasp and easy to use: no annotations, no XML configurations;
just plain, comprehensible PHP code - and we believe that Opis ORM trully delivers those things.
{% endcapture %}
{% include section/description.html
title="Focused on developers"
invert=false
img="developer"
content=about_section %}

{% include section/modern.html name=page.project.title %}