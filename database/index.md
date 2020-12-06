---
layout: project
title: Opis Database | PHP Library
description: A PHP library built around PDO, that provides a powerful query builder and an easy to use schema builder
    for MySQL, SQLite, PostgreSQL and other RDBMS
canonical: /database/
ga: true
project:
    title: Opis Database
    description: Database abstraction layer
    docs_url: https://docs.opis.io/database/4.x
    github: https://github.com/opis/database
    artwork: database
    license: Apache 2.0
---
{% capture about_section %}
Opis Database aims to provide a unified way of interacting with databases,
regardless of the relational database management system you are currently using.
{% endcapture %}
{% include section/lead.html
title="One codebase, multiple RDBMS"
icon=page.project.artwork
content=about_section %}

{% capture about_section %}
The library is shipped with an expressive query builder which heavily relies on semantics
for making sure that the code you write will be easy to follow by every member of your team.
{% endcapture %}
{% include section/description.html
title="Expressive query builder"
invert=false
img="terminal"
content=about_section %}

{% include section/modern.html name=page.project.title %}