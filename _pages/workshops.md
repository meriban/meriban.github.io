---
layout: single
title: Workshops
author_profile: true
permalink: /workshops/
---
{%- for workshop in site.workshops -%}

<ul>
    <li>
        <div>
            <p><a href="{{ workshop.url }}"><strong>{{ workshop.heading }}</strong></a>, {{ workshop.location }}, {{ workshop.dates }}</p>
            <p>{{workshop.description }}</p>
        </div>
    </li>
</ul>


{%- endfor -%}