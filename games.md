---
title: Games
layout: page
---
{% for game in site.games %}
  <h2>
    <a href="{{ game.url }}">
      {{ game.title }}
    </a>
  </h2>
{% endfor %}
