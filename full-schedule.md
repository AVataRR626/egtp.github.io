---
layout: content
title: Full schedule
eyebrow: EGTP 2026 programme
permalink: /full-schedule/
wide: true
full_width: true
show_unconfirmed_details: false
---

All talks across the three streams are shown below. Unconfirmed programme slots are marked “To Be Confirmed” and remain subject to change. Select a confirmed talk title for its full description and speaker information.

Check the individual [Tabletop]({{ '/tabletop/' | relative_url }}), [Digital]({{ '/digital/' | relative_url }}) and [Academic]({{ '/academic/' | relative_url }}) stream pages for focused schedules.

{% assign tabletop_talks = site.talks | where: "stream", "Tabletop" | sort: "time" %}
{% assign digital_talks = site.talks | where: "stream", "Digital" | sort: "time" %}
{% assign academic_talks = site.talks | where: "stream", "Academic" | sort: "time" %}
{% assign before_break = "13:00,13:15,13:30,13:45,14:00,14:15,14:30,14:45,15:00,15:15,15:30,15:45" | split: "," %}
{% assign after_break = "17:00,17:15,17:30,17:45,18:00,18:15,18:30,18:45,19:00,19:15" | split: "," %}

<div class="schedule-wrap full-schedule-wrap">
  <table class="full-schedule">
    <colgroup>
      <col class="full-schedule-time">
      <col class="full-schedule-stream-column">
      <col class="full-schedule-stream-column">
      <col class="full-schedule-stream-column">
    </colgroup>
    <thead>
      <tr>
        <th scope="col">Time</th>
        <th class="full-schedule-heading-tabletop" scope="col"><span>Tabletop</span><small>Room A</small></th>
        <th class="full-schedule-heading-digital" scope="col"><span>Digital</span><small>Room B</small></th>
        <th class="full-schedule-heading-academic" scope="col"><span>Academic</span><small>Room C</small></th>
      </tr>
    </thead>
    <tbody>
      <tr class="full-schedule-shared-row">
        <th scope="row">12–1pm</th>
        <td colspan="3"><strong>Doors open and networking</strong></td>
      </tr>
      {% assign tabletop_remaining = 0 %}
      {% assign digital_remaining = 0 %}
      {% assign academic_remaining = 0 %}
      {% for slot in before_break %}
        <tr class="full-schedule-time-row">
          <th scope="row">{% include format-time.html time=slot %}</th>
          {% if tabletop_remaining > 0 %}
            {% assign tabletop_remaining = tabletop_remaining | minus: 1 %}
          {% else %}
            {% assign tabletop_talk = tabletop_talks | where: "time", slot | first %}
            {% assign tabletop_span = tabletop_talk.duration | remove: " min" | plus: 0 | divided_by: 15 %}
            {% if tabletop_talk and tabletop_span < 1 %}{% assign tabletop_span = 1 %}{% endif %}
            {% include full-schedule-cell.html talk=tabletop_talk talks=tabletop_talks slot=slot stream="tabletop" rowspan=tabletop_span %}
            {% assign tabletop_remaining = tabletop_span | minus: 1 %}
          {% endif %}
          {% if digital_remaining > 0 %}
            {% assign digital_remaining = digital_remaining | minus: 1 %}
          {% else %}
            {% assign digital_talk = digital_talks | where: "time", slot | first %}
            {% assign digital_span = digital_talk.duration | remove: " min" | plus: 0 | divided_by: 15 %}
            {% if digital_talk and digital_span < 1 %}{% assign digital_span = 1 %}{% endif %}
            {% include full-schedule-cell.html talk=digital_talk talks=digital_talks slot=slot stream="digital" rowspan=digital_span %}
            {% assign digital_remaining = digital_span | minus: 1 %}
          {% endif %}
          {% if academic_remaining > 0 %}
            {% assign academic_remaining = academic_remaining | minus: 1 %}
          {% else %}
            {% assign academic_talk = academic_talks | where: "time", slot | first %}
            {% assign academic_span = academic_talk.duration | remove: " min" | plus: 0 | divided_by: 15 %}
            {% if academic_talk and academic_span < 1 %}{% assign academic_span = 1 %}{% endif %}
            {% include full-schedule-cell.html talk=academic_talk talks=academic_talks slot=slot stream="academic" rowspan=academic_span %}
            {% assign academic_remaining = academic_span | minus: 1 %}
          {% endif %}
        </tr>
      {% endfor %}
      <tr class="full-schedule-break-row">
        <th scope="row">4:00pm</th>
        <td colspan="3"><strong>Break and networking across all streams · talks resume at 5:00pm</strong></td>
      </tr>
      {% assign tabletop_remaining = 0 %}
      {% assign digital_remaining = 0 %}
      {% assign academic_remaining = 0 %}
      {% for slot in after_break %}
        <tr class="full-schedule-time-row">
          <th scope="row">{% include format-time.html time=slot %}</th>
          {% if tabletop_remaining > 0 %}
            {% assign tabletop_remaining = tabletop_remaining | minus: 1 %}
          {% else %}
            {% assign tabletop_talk = tabletop_talks | where: "time", slot | first %}
            {% assign tabletop_span = tabletop_talk.duration | remove: " min" | plus: 0 | divided_by: 15 %}
            {% if tabletop_talk and tabletop_span < 1 %}{% assign tabletop_span = 1 %}{% endif %}
            {% include full-schedule-cell.html talk=tabletop_talk talks=tabletop_talks slot=slot stream="tabletop" rowspan=tabletop_span %}
            {% assign tabletop_remaining = tabletop_span | minus: 1 %}
          {% endif %}
          {% if digital_remaining > 0 %}
            {% assign digital_remaining = digital_remaining | minus: 1 %}
          {% else %}
            {% assign digital_talk = digital_talks | where: "time", slot | first %}
            {% assign digital_span = digital_talk.duration | remove: " min" | plus: 0 | divided_by: 15 %}
            {% if digital_talk and digital_span < 1 %}{% assign digital_span = 1 %}{% endif %}
            {% include full-schedule-cell.html talk=digital_talk talks=digital_talks slot=slot stream="digital" rowspan=digital_span %}
            {% assign digital_remaining = digital_span | minus: 1 %}
          {% endif %}
          {% if academic_remaining > 0 %}
            {% assign academic_remaining = academic_remaining | minus: 1 %}
          {% else %}
            {% assign academic_talk = academic_talks | where: "time", slot | first %}
            {% assign academic_span = academic_talk.duration | remove: " min" | plus: 0 | divided_by: 15 %}
            {% if academic_talk and academic_span < 1 %}{% assign academic_span = 1 %}{% endif %}
            {% include full-schedule-cell.html talk=academic_talk talks=academic_talks slot=slot stream="academic" rowspan=academic_span %}
            {% assign academic_remaining = academic_span | minus: 1 %}
          {% endif %}
        </tr>
      {% endfor %}
      <tr class="full-schedule-shared-row">
        <th scope="row">7:30pm</th>
        <td colspan="3"><strong>Networking · venue closes at 8:30pm</strong></td>
      </tr>
      <tr class="full-schedule-time-row full-schedule-close-row">
        <th scope="row">8:30pm</th>
        <td colspan="3"><strong>Venue close</strong></td>
      </tr>
    </tbody>
  </table>
</div>

Programme details may change. 