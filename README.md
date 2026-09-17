# git-lex-www

> Official website and documentation portal for [Git-lex](https://git-lex.com) — Git extensions for knowledge graphs.

This repository powers [`https://git-lex.com`](https://git-lex.com), hosted via GitHub Pages with Jekyll. It provides the landing page, interactive graph documentation, getting started guides, and the [Subtexture](https://git-lex.com/subtexture/) persistent agent portal.

---

## Site Structure

```text
git-lex-www/
├── _config.yml               # Jekyll configuration and collection settings
├── CNAME                     # Custom domain (git-lex.com)
├── Gemfile                   # Ruby gem dependencies
├── index.html                # Main git-lex.com landing page
├── _docs/                    # Documentation articles (/docs/:title/)
│   ├── getting-started.md    # Installation, repository init, creation, querying
│   ├── architecture.md       # One-graph model, RDF 1.2, OxiGraph engine
│   └── document-types.md     # Document class schemas, frontmatter, and kits
├── subtexture/               # Subtexture portal (/subtexture/)
│   └── index.html            # Interactive SVG stack diagram & 8 subsystem guide
├── _layouts/                 # Jekyll templates (default, docs, subtexture)
├── _includes/                # Reusable navigation, head, and footer components
└── assets/                   # CSS stylesheets, fonts, and JavaScript assets
```

---

## Local Development

### Prerequisites
- Ruby (>= 3.0)
- Bundler (`gem install bundler`)

### Running Locally

```bash
# 1. Clone the repository
git clone https://github.com/repolex-ai/git-lex-www.git
cd git-lex-www

# 2. Install Ruby dependencies
bundle install

# 3. Start local development server
bundle exec jekyll serve

# 4. Open browser
# Navigate to http://localhost:4000
```

To live-reload on draft changes:

```bash
bundle exec jekyll serve --livereload
```

---

## Content Authoring Guidelines

- **Documentation Collection (`_docs/`)**:
  Articles in `_docs/` use YAML frontmatter with `title`, `order`, and `nav_title`. They are automatically rendered into the documentation sidebar based on `order`.
- **Subtexture Portal (`subtexture/`)**:
  The subtexture page uses the dedicated `subtexture` layout (`layout: subtexture`). It details the 8 persistent agent subsystems (git-lex, kits, pan, horae, ravel, copia, syrinx, iris/arke).
- **Relative URLs**:
  Always use `{{ site.baseurl }}` or relative paths when linking assets and pages so links resolve correctly in development and production.

---

## Deployment

The site is configured with GitHub Pages. Commits pushed to `main` trigger the GitHub Pages build and deploy pipeline automatically.

---

## License

MIT License. Developed for the [Repolex](https://repolex.ai) ecosystem.
