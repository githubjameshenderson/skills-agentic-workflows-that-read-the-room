---
name: update-github-info
description: Review recent official GitHub updates and propose a concise website refresh for Mona.
intent: Keep GitHub Info accurate and useful for developers with current official updates, reviewed by Mona.
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
  copilot-requests: write
strict: true
engine:
  id: copilot
  copilot-sdk: true
tools:
  bash: false
  cli-proxy: false
  edit:
  web-fetch:
network:
  allowed:
    - github.blog
    - github.com
safe-outputs:
  create-pull-request:
    title-prefix: "Update GitHub Info: "
    allowed-files:
      - site/content/github-info.md
    base-branch: main
    draft: false
---

# Update GitHub Info

Read `notes/mona-notes.md` and the current `site/content/github-info.md`.
Fetch both `https://github.blog/latest/` and
`https://github.blog/changelog/` with the web-fetch tool.

Use only current, relevant information confirmed by those official sources.
Keep changes short and practical for developers, preserve useful existing
guidance, and cite each blog or changelog item with its title and source link.
Update only `site/content/github-info.md`.

If either source cannot be fetched, the notes or page cannot be read, or there
is no meaningful update, make no changes and call `noop` with a brief reason.
Otherwise, edit the page and use the configured `create-pull-request` safe
output to open one non-draft PR against `main`. In the PR body, summarize the
change, list its official sources, and ask Mona to review it. Never write
directly to `main` or use another mechanism to publish changes.
