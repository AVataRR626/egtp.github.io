# Every Game Talk Possible

A small Jekyll site for Every Game Talk Possible (EGTP), a Sydney Games Festival event.

## Local preview

```sh
bundle install
bundle exec jekyll serve
```

For live reload on non-standard ports:

```sh
bundle exec jekyll serve --livereload --port 4100 --livereload-port 35730
```

A fully populated multi-speaker example is available at `/sample-talk/`. Its source is `sample-talk.md`; because it sits outside `_talks`, it does not appear in the live programme or speaker directory.


## Updating the programme

The **Selected Talks** and **Selected Speakers** tabs in the [programme spreadsheet](https://docs.google.com/spreadsheets/d/12Y6ny5lC_O6qsiXLjnArOfeu3Y-g82PHULys3gr4t7w/edit) are the primary sources of talk and speaker data for this project, unless a request explicitly says otherwise. Use the runsheet and Call for Speakers responses to reconcile differences or fill gaps.

The stream pages are the Markdown files `digital.md`, `academic.md`, `tabletop.md` and `lightning-talks.md`. Replace the “to be announced” entries with confirmed talks as the programme is finalised.

Each talk is a Markdown file in `_talks`. Its front matter stores the title, summary, stream, time, duration and a list of one or more speakers. Stream schedules, individual talk pages and the speaker directory are generated from this collection.

Name files with the stream code plus one or two keywords from the title: `TT-` for Tabletop, `DI-` for Digital and `AC-` for Academic. For example: `DI-open-source.md`. Do not include the speaker name or scheduled time in the filename.

Use this shape for a new talk:

```yaml
---
title: "Talk title"
summary: "Short talk summary."
stream: "Digital"
status: "confirmed"
time: "13:00"
duration: "30 min"
speakers:
  - name: "Speaker name"
    personal_url: "https://example.com"
    social_links:
      - label: "LinkedIn"
        url: "https://www.linkedin.com/in/speaker-name/"
    job_title: "Job title"
    org: "Organisation"
    org_url: "https://example.org"
    photo: "speaker-name.jpg"
    bio: "Short speaker biography."
---
```

Set `status` to either `confirmed` or `unconfirmed`. Only confirmed talks appear in public stream schedules and the speaker directory. Unconfirmed talks are collected at `/unconfirmed-talks/`, which is intentionally omitted from site navigation.

Speaker photos belong in `assets/images/speakers`. Leave optional values blank until they are confirmed.

Ticket and social links are configured once in `_config.yml` and reused across the site.
