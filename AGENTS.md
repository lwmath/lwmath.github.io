# AGENTS.md

This repository contains Ling Wang's academic homepage.

- Site: https://lwmath.github.io
- Repository: `lwmath/lwmath.github.io`
- Default branch: `master`
- Framework: Jekyll / AcademicPages / Minimal Mistakes
- Primary language of the website: English

The goal of maintenance is to keep the website accurate, lightweight, stable, and easy to update.

Do not redesign the site unless explicitly requested.

## 1. General working principles

When modifying this repository:

1. Inspect the relevant existing files before editing.
2. Prefer minimal changes over broad refactors.
3. Preserve the current visual style unless a redesign is explicitly requested.
4. Preserve all existing content unless the user explicitly asks to change or remove it.
5. Do not invent factual information.
6. Do not invent publication metadata, talk abstracts, dates, institutions, links, journal information, or affiliations.
7. Prefer data-driven solutions over duplicated hard-coded content.
8. Avoid unnecessary dependencies, plugins, frameworks, or JavaScript.
9. Keep the site compatible with GitHub Pages.
10. Before finishing, check syntax and likely rendering behavior.

When there are several possible implementations, prefer the simplest implementation that is robust and maintainable.

## 2. Important repository architecture

### Talks

Single source of truth:

```text
_data/talks.yml
```

Shared rendering template:

```text
_includes/talk-item.html
```

Upcoming talks on the homepage:

```text
_includes/upcoming-talks.html
```

Talk archive:

```text
_pages/talks.md
```

Talk styling:

```text
assets/css/custom.css
```

Do not duplicate talk information manually in About or Talks pages.

### Publications

Single source of truth:

```text
_data/publications.yml
```

Shared rendering template:

```text
_includes/publication-item.html
```

Chronological view:

```text
_includes/pubs-by-time.html
```

Topic view:

```text
_includes/pubs-by-topic.html
```

Selected publications:

```text
_includes/selected-publications.html
```

Main page:

```text
_pages/publications.html
```

Do not manually duplicate publication entries elsewhere.

### Homepage

Main homepage:

```text
_pages/about.md
```

The homepage currently contains:

- automatic last-updated date using `site.time`
- short biography
- CV link
- automatically generated Upcoming Talks
- Selected Publications

Do not manually hard-code upcoming talks into `about.md`.

## 3. Talks data conventions

Each talk in `_data/talks.yml` should use the following structure when applicable:

```yaml
- title: "Talk title"
  date: "YYYY-MM-DD"
  end_date: "YYYY-MM-DD"
  category: seminar
  event: "Seminar or conference name"
  institution: "Institution name"
  location: "City, Country"
  note: "Optional note"
  abstract: >-
    Optional abstract text.
```

Only include fields that are actually needed.

### Dates

Use ISO format:

```text
YYYY-MM-DD
```

For multi-day conferences, use:

```yaml
date: "YYYY-MM-DD"
end_date: "YYYY-MM-DD"
```

The start date is used for ordering.

The end date is used to decide whether a talk is still upcoming.

### Talk categories

Use only:

```yaml
category: seminar
```

or

```yaml
category: conference
```

Use `seminar` for seminars, colloquia, research forums, and invited departmental talks.

Use `conference` for conferences, workshops, and conference-style meetings.

Do not create new categories without a good reason.

### Talk titles

Use sentence case, not title case.

Preferred:

```text
A brief introduction to anisotropic minimal surfaces
```

Avoid:

```text
A Brief Introduction to Anisotropic Minimal Surfaces
```

Preserve mathematical capitalization where needed.

Use typographically correct names such as:

```text
Monge–Ampère
Allen–Cahn
Simon–Smith
```

### Talk abstracts

Abstracts are optional.

If the original abstract is known, store it in:

```yaml
abstract: >-
```

Do not invent an abstract.

If no reliable abstract is available, omit the `abstract` field entirely.

Do not create an empty abstract field.

Preserve mathematical content carefully.

Use MathJax inline delimiters where mathematics is needed:

```text
\( ... \)
```

Example:

```yaml
abstract: >-
  We prove that every stable solution in \(\mathbb{R}^3\) is planar.
```

Avoid unnecessary Markdown inside abstracts.

The rendering template should output:

```liquid
{{ talk.abstract }}
```

rather than:

```liquid
{{ talk.abstract | markdownify }}
```

unless there is a specific reason to restore Markdown processing.

This avoids interference between Kramdown and mathematical LaTeX.

## 4. Abstract toggle UI

Talk abstracts should be collapsible.

Preferred layout for future implementation:

```text
Talk title   ▸ Abstract
Event, Institution, Location — Date
```

When expanded:

```text
Talk title   ▾ Abstract
Event, Institution, Location — Date

│ Abstract text...
```

The Abstract control belongs semantically with the talk title.

The abstract body should remain below the metadata, so a long abstract does not separate the title from the event/date information.

If implementing this interaction with JavaScript:

- do not use fixed IDs for individual talks
- use classes and the nearest `.talk-item`
- make the interaction work for cloned talk nodes
- preserve accessibility with `aria-expanded`
- make the control keyboard accessible
- do not require external JavaScript libraries

The About page clones `.talk-item` nodes into Upcoming Talks, so code must continue to work after cloning.

## 5. Upcoming Talks behavior

`_includes/upcoming-talks.html` dynamically determines upcoming talks in the browser.

A talk is considered current/upcoming when:

```text
end_date >= today
```

where `end_date` defaults to `date`.

This is intentional.

For a multi-day conference, the talk should remain in Upcoming Talks through the last conference day.

Upcoming talks should be sorted from nearest to farthest.

The Upcoming Talks section should be completely hidden when there are no upcoming talks.

Do not introduce year headings in the Upcoming Talks section.

## 6. Talks archive behavior

`_pages/talks.md` should contain only past talks in the dynamic view.

Past means:

```text
end_date < today
```

The archive is divided into:

```text
Conference and Workshop Talks
```

and

```text
Seminar and Colloquium Talks
```

Within each category:

- newest talks first
- no year grouping

Do not reintroduce year headings unless explicitly requested.

## 7. Publications data conventions

The current publication topics are:

```yaml
topics:
  - key: monge-ampere
    title: "Monge–Ampère Equations"

  - key: fourth-order
    title: "Monge–Ampère Type Fourth-Order Equations"

  - key: geometric-variational
    title: "Geometric and Variational Problems"

  - key: elliptic-problems
    title: "Local and Nonlocal Elliptic Problems"

  - key: others
    title: "Other Topics"
```

Do not create a new publication category for a single paper unless clearly justified.

Prefer fitting new work into this existing medium-granularity structure.

### Publication item format

Typical structure:

```yaml
- title: "Publication title"
  year: 2026
  topic: monge-ampere
  selected: false
  authors:
    - name: "A. Author"
      url: "https://..."
  citation: "Preprint (2026)."
  doi: "https://..."
  pdf: "/files/file.pdf"
  arxiv: "https://arxiv.org/abs/..."
```

The website automatically renders Ling Wang as the site author context, so the `authors` field contains collaborators only.

Do not insert Ling Wang into the collaborators list unless the rendering architecture changes.

Use official or stable academic homepage links for collaborators when available.

Prefer:

1. personal academic homepage
2. official university faculty page
3. official institute profile

Avoid unstable aggregator links unless necessary.

### Publication ordering

`_includes/pubs-by-time.html` renders publications in the order they appear in `_data/publications.yml`.

Therefore, when adding a new publication, place it in the appropriate chronological position.

Normally newest papers should appear first.

Do not blindly sort only by `year` if the intended order within a year is known.

### Selected publications

The field:

```yaml
selected: true
```

controls inclusion in Selected Publications.

Do not automatically mark a new paper as selected.

Preserve the existing selected set unless explicitly asked to change it.

Use explicit:

```yaml
selected: false
```

for non-selected papers when editing entries, for clarity.

## 8. Publication links

Use:

```yaml
doi:
pdf:
arxiv:
```

only when the corresponding link is known and verified.

Do not invent an arXiv number.

Local PDFs should normally use:

```text
/files/FILENAME.pdf
```

Do not use absolute `http://lwmath.github.io/...` URLs for local files when a relative site path works.

## 9. Styling conventions

Custom styles belong primarily in:

```text
assets/css/custom.css
```

Do not scatter new CSS into individual Markdown pages unless there is a strong reason.

Reuse existing classes where appropriate.

Keep styling subtle and academic.

Avoid oversized buttons, bright colors, heavy animations, unnecessary cards, and large UI frameworks.

Controls such as Abstract toggles should be visually secondary to the academic content.

Mobile behavior must remain usable.

## 10. JavaScript and HTML compression

This site has:

```yaml
compress_html:
  clippings: all
```

in `_config.yml`.

This is important.

Avoid JavaScript line comments of the form:

```javascript
// comment
```

inside page scripts, because HTML compression may remove line breaks and cause the comment to swallow subsequent code.

Prefer:

```javascript
/* comment */
```

or no JavaScript comments.

Keep page scripts simple.

Do not assume whitespace will be preserved.

## 11. Jekyll and GitHub Pages compatibility

The site uses GitHub Pages-compatible plugins.

Do not add unsupported Jekyll plugins without explicitly checking GitHub Pages compatibility.

Current configuration uses standard plugins such as:

- jekyll-feed
- jekyll-gist
- jekyll-paginate
- jekyll-sitemap
- jekyll-redirect-from
- jemoji

Avoid adding plugins merely to solve something that can be done with Liquid, HTML, CSS, or small JavaScript.

## 12. Last updated date

The homepage currently uses:

```liquid
{{ site.time | date: "%B %-d, %Y" }}
```

This means the displayed date is the site build time.

That behavior is intentional unless explicitly changed.

Remember that `_config.yml` currently uses:

```yaml
timezone: Etc/UTC
```

Do not silently change the timezone.

If changing timezone is requested, discuss whether `Europe/Rome` or another timezone is appropriate.

## 13. Legacy/template content

This repository originated from AcademicPages and may contain unused or demo content.

Examples may include:

- old `_publications/`
- old `_talks/`
- demo teaching entries
- demo portfolio entries
- demo posts
- old JSON CV content
- template README material

Do not assume these are authoritative sources for current website content.

For current talks and publications, prefer:

```text
_data/talks.yml
_data/publications.yml
```

Do not migrate or delete large amounts of legacy material unless explicitly requested.

## 14. Content accuracy

For academic content:

- preserve mathematical notation exactly
- preserve publication titles accurately
- preserve journal names and bibliographic data
- verify externally when the user explicitly asks for verification
- never fabricate missing metadata

For talk abstracts found online:

- use the exact official abstract when appropriate and permitted
- otherwise summarize only if explicitly requested
- distinguish a paper abstract from a talk abstract
- do not silently substitute a paper abstract for a talk abstract

## 15. Editing strategy

For routine updates such as a new talk or publication:

1. inspect the relevant data file
2. make the smallest necessary data change
3. inspect the renderer only if the requested behavior requires it
4. avoid changing unrelated files
5. validate YAML
6. validate Liquid/HTML syntax
7. check JavaScript logic if affected
8. check mobile implications if CSS changes
9. review the diff for accidental unrelated edits

## 16. Validation before completion

Before reporting that a task is finished, perform all applicable checks.

At minimum:

- YAML parses successfully
- edited files contain no obvious indentation errors
- Liquid tags are balanced
- HTML nesting is valid enough for browsers/Jekyll
- JavaScript has no obvious syntax errors
- no duplicated IDs were introduced
- relative URLs are correct
- publication topic keys match defined topic keys
- talk categories are `seminar` or `conference`
- dates use ISO format
- optional fields are omitted rather than left malformed

If the environment supports a Jekyll build, run it.

If a full Jekyll build is unavailable, say exactly what validation was performed instead.

Never claim a build passed unless it was actually run.

## 17. Git workflow

This is a personal academic homepage repository.

The default workflow is to work directly on the `master` branch.

For routine maintenance:

1. inspect the latest `master`
2. make the smallest necessary change
3. validate the modified files
4. inspect the complete diff
5. create one focused commit
6. push directly to `origin/master`

Do not create branches or pull requests unless explicitly requested.

Never:

- force-push
- rewrite published history
- push unvalidated changes when validation is available
- delete unrelated files
- combine unrelated maintenance tasks into one commit

If validation fails, fix the problem before pushing.

If a pushed change later proves incorrect, prefer a normal corrective commit or `git revert` rather than rewriting history.

## 18. Scope control

Do not make unrelated cleanup changes during a focused task.

For example, when adding a new talk, do not also rewrite the homepage biography, change publication categories, redesign navigation, remove template files, change site fonts, or change SEO settings unless explicitly requested.

If you notice a separate issue, mention it after completing the requested task rather than silently modifying it.

## 19. User preference

The site owner prefers:

- direct, maintainable solutions
- complete and correct edits
- minimal unnecessary redesign
- mathematically accurate notation
- centralized data
- preserving all relevant details
- avoiding repeated manual edits across multiple files

When possible, solve a recurring maintenance problem once at the template/data level rather than requiring repeated page-level edits.

## 20. Final reporting format

After completing a task, report briefly:

1. files changed
2. what was changed
3. validation performed
4. commit hash
5. whether the push to `master` succeeded
6. anything that still requires confirmation

Do not claim that a commit or push succeeded unless it actually succeeded.
If a change has not actually been committed or pushed, say so explicitly.
