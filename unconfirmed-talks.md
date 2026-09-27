---
layout: content
title: Unconfirmed talks
eyebrow: Programme review
permalink: /unconfirmed-talks/
sitemap: false
wide: true
---

{% assign unconfirmed_talks = site.talks | where: "status", "unconfirmed" | sort: "time" %}

{% if unconfirmed_talks.size == 0 %}
All talks are currently confirmed.
{% else %}
<div class="schedule-wrap">
  <table class="stream-schedule">
    <thead>
      <tr>
        <th scope="col">Time</th>
        <th scope="col">Stream</th>
        <th scope="col">Speaker</th>
        <th scope="col">Topic / title</th>
      </tr>
    </thead>
    <tbody>
      {% for talk in unconfirmed_talks %}
        <tr>
          <th scope="row">{% include format-time.html time=talk.time %}</th>
          <td>{{ talk.stream }}</td>
          <td>
            {% for speaker in talk.speakers %}
              {{ speaker.name }}{% unless forloop.last %}<br>{% endunless %}
            {% endfor %}
          </td>
          <td><a href="{{ talk.url | relative_url }}">{{ talk.title }}</a></td>
        </tr>
      {% endfor %}
    </tbody>
  </table>
</div>
{% endif %}
