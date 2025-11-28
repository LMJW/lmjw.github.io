# Tags

This site uses tags to group related posts.

Each blog post can define tags in a YAML front matter block at the top
of the file, for example:

```yaml
---
title: An experiment using golang and python to execute command in docker
tags: [golang, python, docker, subprocess, bash]
date: 2017-11-26
---
```

With the current MkDocs + Material configuration, these tags are shown
near the top of the page as visual labels, making your blog look more
like a professional technical site.

## How to use tags for new posts

When you create a new post under `docs/blog/`, include a `tags` field
in the YAML front matter:

```yaml
---
title: My new post
tags: [notes, rust, grpc]
date: 2025-11-28
---
```

After that:

- The tags will appear on the post page itself.
- The built-in search will help you find posts by tag names and content.

You can freely choose any tags that make sense for your writing (e.g.
`golang`, `python`, `docker`, `kubernetes`, `learning`, `os-dev`).