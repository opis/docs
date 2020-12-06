---
layout: landing-project
title: Opis JSON Schema | PHP Library
description: Opis JSON Schema is a PHP library, fully compliant with the latest JSON Schema specification
    that can be used to validate JSON documents
canonical: /json-schema/
ga: true
project:
    title: Opis JSON Schema
    description: Validate JSON documents
    docs_url: /json-schema/1.x/
    github: https://github.com/opis/json-schema
    artwork: schema
    license: Apache 2.0
---
{% capture about_section %}
The library implements the latest specification (<a href="http://json-schema.org/" target="_blank">draft-7</a>)
of the JSON Schema standard and provides a comprehensive documentation
that covers both the standard itself and the library's own API.
{% endcapture %}
{% include section/lead.html
title="JSON Schema compliant"
icon=page.project.artwork
content=about_section %}

{% capture about_section %}
Opis Schema help developers build reliable and secure RESTful APIs,
by allowing them to define validation rules for user-input data, using only JSON documents.
This way, your app becomes more maintainable and less error-prone.
{% endcapture %}
{% include section/description.html
title="Perfect for building RESTful APIs"
invert=false
img="build"
content=about_section %}

{% include section/modern.html name=page.project.title %}