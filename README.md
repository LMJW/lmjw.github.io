# MyBlog

This site is built with [MkDocs](https://www.mkdocs.org/) and the [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme.

## Writing

All content lives under the `docs/` directory in this repository.

- Home page: `docs/index.md` (this page)
- Blog index: `docs/blog/index.md`
- Blog posts: individual files under `docs/blog/`, for example `docs/blog/hello-world.md`

To add a new post:

1. Create a new Markdown file under `docs/blog/`, e.g. `docs/blog/my-new-post.md`.
2. Optionally add a link to it from `docs/blog/index.md`.
3. Commit and push your changes.

## Local preview

From the repository root:

```bash
brew install mkdocs-material # if not installed
mkdocs build --strict
mkdocs serve
```

Then open http://127.0.0.1:8000 in your browser.