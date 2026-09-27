---
layout: content
title: Speakers
eyebrow: EGTP 2026 programme
permalink: /speakers/
wide: true
---

{% assign ordered_talks = site.pages | where: "layout", "__talk_collection_fallback__" %}
{% if site.talks %}
  {% assign confirmed_talks = site.talks | where: "status", "confirmed" %}
  {% assign ordered_talks = confirmed_talks | sort: "time" %}
{% endif %}
{% assign seen_speakers = "|" %}

<div class="speaker-filter-bar">
  <p class="speaker-filter-label">Filter by stream</p>
  <div class="speaker-filters" role="group" aria-label="Filter speakers by stream">
    <button class="speaker-filter-button is-active" type="button" data-speaker-filter="all" aria-pressed="true">All <span class="speaker-filter-count"></span></button>
    <button class="speaker-filter-button" type="button" data-speaker-filter="tabletop" aria-pressed="false">Tabletop <span class="speaker-filter-count"></span></button>
    <button class="speaker-filter-button" type="button" data-speaker-filter="digital" aria-pressed="false">Digital <span class="speaker-filter-count"></span></button>
    <button class="speaker-filter-button" type="button" data-speaker-filter="academic" aria-pressed="false">Academic <span class="speaker-filter-count"></span></button>
  </div>
  <p class="speaker-filter-status" aria-live="polite"></p>
</div>

<div class="speaker-directory" data-placeholder-yellow="{{ '/assets/images/speakers/speaker-placeholder-yellow.svg' | relative_url }}" data-placeholder-green="{{ '/assets/images/speakers/speaker-placeholder-teal.svg' | relative_url }}">
  {% for talk in ordered_talks %}
    {% for speaker in talk.speakers %}
      {% assign speaker_key = speaker.name | slugify | prepend: "|" | append: "|" %}
      {% unless seen_speakers contains speaker_key %}
        {% assign speaker_slug = speaker.name | slugify %}
        {% assign speaker_name_parts = speaker.name | split: " " %}
        {% assign speaker_surname = speaker_name_parts | last | downcase %}
        {% assign speaker_streams = "" %}
        {% for speaker_talk in ordered_talks %}
          {% for talk_speaker in speaker_talk.speakers %}
            {% if talk_speaker.name == speaker.name %}
              {% assign speaker_stream_slug = speaker_talk.stream | slugify %}
              {% unless speaker_streams contains speaker_stream_slug %}
                {% capture speaker_streams %}{{ speaker_streams }} {{ speaker_stream_slug }}{% endcapture %}
              {% endunless %}
            {% endif %}
          {% endfor %}
        {% endfor %}
        <article class="speaker-directory-card" data-speaker-streams="{{ speaker_streams | strip }}" data-speaker-sort="{{ speaker_surname | escape }} {{ speaker.name | downcase | escape }}" tabindex="0" aria-labelledby="speaker-{{ speaker_slug }}">
          <div class="speaker-card-front">
            {% if speaker.photo != empty %}
              {% assign speaker_photo = speaker.photo %}
            {% else %}
              {% assign speaker_photo = "speaker-placeholder-yellow.svg" %}
            {% endif %}
            <img class="speaker-directory-photo" src="{{ '/assets/images/speakers/' | append: speaker_photo | relative_url }}" alt=""{% if speaker.photo == empty %} data-speaker-placeholder{% endif %}>
            <div class="speaker-card-caption">
              <h2 id="speaker-{{ speaker_slug }}">{{ speaker.name }}</h2>
              {% if speaker.job_title != empty or speaker.org != empty %}
                <p class="speaker-role">
                  {% if speaker.job_title != empty %}{{ speaker.job_title }}{% endif %}
                  {% if speaker.job_title != empty and speaker.org != empty %} · {% endif %}
                  {{ speaker.org }}
                </p>
              {% endif %}
            </div>
          </div>
          <div class="speaker-card-reveal">
            {% if speaker.bio != empty %}
              <h3>About {{ speaker.name }}</h3>
              <p>{{ speaker.bio }}</p>
            {% endif %}
            {% if speaker.org_url != empty %}
              <p class="speaker-org-link"><a href="{{ speaker.org_url }}" target="_blank" rel="noopener">Visit {{ speaker.org }}</a></p>
            {% endif %}
            {% include speaker-links.html speaker=speaker %}
            <h3 class="speaker-talk-heading">{% assign speaker_talk_count = 0 %}{% for speaker_talk in ordered_talks %}{% for talk_speaker in speaker_talk.speakers %}{% if talk_speaker.name == speaker.name %}{% assign speaker_talk_count = speaker_talk_count | plus: 1 %}{% endif %}{% endfor %}{% endfor %}{% if speaker_talk_count == 1 %}Talk{% else %}Talks{% endif %}</h3>
            <ul class="speaker-talk-list">
              {% for speaker_talk in ordered_talks %}
                {% for talk_speaker in speaker_talk.speakers %}
                  {% if talk_speaker.name == speaker.name %}
                    <li><a href="{{ speaker_talk.url | relative_url }}">{{ speaker_talk.title }}</a> <span>{{ speaker_talk.stream }} · {% include format-time.html time=speaker_talk.time %}</span></li>
                  {% endif %}
                {% endfor %}
              {% endfor %}
            </ul>
          </div>
        </article>
        {% assign seen_speakers = seen_speakers | append: speaker_key %}
      {% endunless %}
    {% endfor %}
  {% endfor %}
</div>

<script>
  (() => {
    const buttons = Array.from(document.querySelectorAll("[data-speaker-filter]"));
    const cards = Array.from(document.querySelectorAll("[data-speaker-streams]"));
    const directory = document.querySelector(".speaker-directory");
    const status = document.querySelector(".speaker-filter-status");

    cards
      .sort((a, b) => a.dataset.speakerSort.localeCompare(b.dataset.speakerSort))
      .forEach((card) => directory.appendChild(card));

    const applyAlternatingColours = (visibleCards) => {
      visibleCards.forEach((card, index) => {
        const useGreen = index % 2 === 1;
        card.classList.toggle("speaker-card-green", useGreen);
        const placeholder = card.querySelector("[data-speaker-placeholder]");
        if (placeholder) {
          placeholder.src = useGreen ? directory.dataset.placeholderGreen : directory.dataset.placeholderYellow;
        }
      });
    };

    const matchesFilter = (card, filter) => {
      if (filter === "all") return true;
      return card.dataset.speakerStreams.split(/\s+/).includes(filter);
    };

    buttons.forEach((button) => {
      const filter = button.dataset.speakerFilter;
      const count = cards.filter((card) => matchesFilter(card, filter)).length;
      button.querySelector(".speaker-filter-count").textContent = `(${count})`;
    });

    const applyFilter = (filter) => {
      let visibleCount = 0;
      const visibleCards = [];
      cards.forEach((card) => {
        const visible = matchesFilter(card, filter);
        card.hidden = !visible;
        if (visible) {
          visibleCount += 1;
          visibleCards.push(card);
        }
      });
      applyAlternatingColours(visibleCards);
      buttons.forEach((button) => {
        const active = button.dataset.speakerFilter === filter;
        button.classList.toggle("is-active", active);
        button.setAttribute("aria-pressed", active.toString());
      });
      const label = filter === "all" ? "all streams" : `${filter} stream`;
      status.textContent = `Showing ${visibleCount} ${visibleCount === 1 ? "speaker" : "speakers"} from ${label}.`;
    };

    buttons.forEach((button) => {
      button.addEventListener("click", () => applyFilter(button.dataset.speakerFilter));
    });

    applyFilter("all");
  })();
</script>
