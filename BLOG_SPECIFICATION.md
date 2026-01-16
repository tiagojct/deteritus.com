# Blog Specification

This document describes all features and design specifications for recreating this blog. Originally built with Zola, this specification is platform-agnostic and can be used to recreate the site in Hugo or any other static site generator.

## Overview

A minimalist personal blog with multiple post types, reviews with ratings, favorites system, and a clean typography-focused design.

---

## Site Configuration

### Basic Settings
- **Title**: Deteritus
- **Author**: Tiago Jacinto
- **Language**: English (en)
- **Features**: RSS feed, Search, Tags taxonomy

### Navigation
- **Header**: Logo/title linking to home
- **Nav Links** (with SVG icons):
  - Archive (archive icon)
  - Search (magnifying glass icon)
  - About (person icon)
- **Footer Links** (with SVG icons):
  - RSS (radio waves icon)
  - Colophon (info circle icon)
  - Mastodon (mastodon logo, with `rel="me"`)

---

## Design System

### Typography
- **Primary Font**: Iowan Old Style (self-hosted, with Georgia as fallback)
- **Font Weights**: 400 (regular), 700 (bold), 900 (black) + italic variants
- **Monospace**: SF Mono, Consolas, monospace
- **Base Font Size**: 22px (desktop), 18px (mobile)
- **Line Height**: 1.7

### Colors (CSS Custom Properties)

**Light Mode:**
```css
--bg: #fffff8;      /* Warm off-white */
--text: #111;       /* Near black */
--muted: #666;      /* Gray for secondary text */
--link: #2a7a4b;    /* Forest green */
--border: #ddd;     /* Light gray */
```

**Dark Mode** (via `prefers-color-scheme: dark`):
```css
--bg: #151515;      /* Dark gray */
--text: #ddd;       /* Light gray */
--muted: #999;      /* Medium gray */
--link: #6ab889;    /* Lighter green */
--border: #333;     /* Dark border */
```

### Layout
- **Max Width**: 44rem (704px)
- **Padding**: 3rem 1.5rem (desktop), 2rem 1rem (mobile)
- **Post Spacing**: 6rem between posts

---

## Post Types

### 1. Regular Post (`post_type: "post"` or omitted)

Standard blog post with title, content, date, and tags.

**Frontmatter:**
```yaml
title: "Post Title"
date: 2024-01-15
tags: ["tag1", "tag2"]
favorite: true  # Optional - shows star icon
```

**Display:**
- Title as h2 link
- Full content
- Date and tags in footer

---

### 2. Quote Post (`post_type: "quote"`)

For sharing quotes without a title.

**Frontmatter:**
```yaml
date: 2024-01-15
tags: ["quotes"]
post_type: "quote"
```

**Display:**
- No title
- Content displayed in blockquote style
- Larger font (1.2rem), green left border
- Date and tags in footer

---

### 3. Link Post (`post_type: "link"`)

For sharing external links with commentary.

**Frontmatter:**
```yaml
title: "Article Title"
date: 2024-01-15
tags: ["links"]
post_type: "link"
link_url: "https://example.com/article"
favorite: true  # Optional
```

**Display:**
- Title links to external URL (with arrow: `→`)
- Title color is link color (green)
- Commentary as content below
- Date and tags in footer

---

### 4. Photo Post (`post_type: "photo"`)

For sharing photos with captions.

**Frontmatter:**
```yaml
title: "Photo Caption Title"
date: 2024-01-15
tags: ["photos"]
post_type: "photo"
photo_url: "/img/my-photo.jpg"
favorite: true  # Optional
```

**Display (List View):**
- Photo displayed first (links to post)
- Caption: "**Title:** content" on same line
- Date and tags in footer

**Display (Single View):**
- Title as h1
- Photo in figure element
- Content below

---

### 5. Album Review (`post_type: "album"`)

For music album reviews with ratings.

**Frontmatter:**
```yaml
title: "Album Name"  # Not displayed, use album field
date: 2024-01-15
tags: ["music", "reviews"]
post_type: "album"
artist: "Artist Name"
album: "Album Title"
year: 1997
rating: "A+"  # Letter grade: A+, A, A-, B+, B, B-, C+, C, C-, D, F
cover_url: "/img/album-cover.jpg"  # Optional
favorite: true  # Optional
```

**Display:**
- Cover image on left (120px list, 180px single)
- Title format: "Artist – Album (Year)" with rating badge
- Content as review text
- Rating displayed as colored badge

---

### 6. Movie Review (`post_type: "movie"`)

For film reviews with ratings.

**Frontmatter:**
```yaml
title: "Movie Title"
date: 2024-01-15
tags: ["film", "reviews"]
post_type: "movie"
year: 2019
rating: "A"
poster_url: "/img/movie-poster.jpg"  # Optional
favorite: true  # Optional
```

**Display:**
- Poster image on left
- Title format: "Title (Year)" with rating badge
- Content as review text

---

### 7. Book Review (`post_type: "book"`)

For book reviews with ratings.

**Frontmatter:**
```yaml
title: "Book Title"
date: 2024-01-15
tags: ["books", "reviews"]
post_type: "book"
author: "Author Name"
year: 1969
rating: "A+"
cover_url: "/img/book-cover.jpg"  # Optional
favorite: true  # Optional
```

**Display:**
- Cover image on left
- Title format: "Author – Title (Year)" with rating badge
- Content as review text

---

## Special Features

### Favorites System

Posts can be marked as favorites with `favorite: true` in frontmatter.

**Visual Indicator:**
- Filled star SVG icon appears after the title
- Star inherits the color of its parent link
- Displayed in list views, single post, and archive

**Star SVG:**
```html
<svg class="star" xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="currentColor" stroke="currentColor" stroke-width="2">
  <polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/>
</svg>
```

---

### Rating System

Reviews use letter grades (A+ through F) displayed as badges.

**Valid Ratings:** A+, A, A-, B+, B, B-, C+, C, C-, D+, D, D-, F

**Badge Styling:**
```css
.rating {
    display: inline-block;
    font-size: 0.7em;
    font-weight: bold;
    padding: 0.15em 0.4em;
    margin-left: 0.5rem;
    background: var(--link);  /* Green background */
    color: var(--bg);         /* White/dark text */
    border-radius: 3px;
    vertical-align: middle;
}
```

---

### Similar Posts

On single post pages, show up to 3 related posts based on shared tags.

**Logic:**
1. Get current post's tags
2. Find other posts that share at least one tag
3. Display up to 3 matches in a 3-column grid
4. Show post type icon + title (with star if favorite)

**Section Title:** "Probably related"

---

### Backlinks

Show which other posts link to the current post.

**Implementation Options:**

**Option A: Build-time (Python script)**
1. Pre-build script scans all markdown files for internal links
2. Generates a JSON file mapping URLs to linking pages
3. Template reads JSON and displays backlinks

**Option B: JavaScript (client-side)**
1. Build script generates a links index JSON file
2. JavaScript on page load fetches index and finds backlinks
3. Dynamically displays "Linked from:" section

**Recommended Implementation (Build-time with Hugo):**

Create a partial that:
1. Iterates through all pages
2. Checks if page content contains link to current page
3. Displays matching pages as backlinks

```go-html-template
{{ $currentURL := .RelPermalink }}
{{ $backlinks := slice }}
{{ range where .Site.RegularPages "RelPermalink" "!=" $currentURL }}
  {{ if findRE $currentURL .RawContent }}
    {{ $backlinks = $backlinks | append . }}
  {{ end }}
{{ end }}

{{ if $backlinks }}
<section class="backlinks">
  <h2>Linked from</h2>
  <ul>
    {{ range $backlinks }}
    <li><a href="{{ .RelPermalink }}">{{ .Title }}</a></li>
    {{ end }}
  </ul>
</section>
{{ endif }}
```

**Styling:**
```css
.backlinks {
    margin-top: 2rem;
    padding-top: 1rem;
    border-top: 1px solid var(--border);
    font-size: 0.85rem;
}

.backlinks h2 {
    font-size: 0.9rem;
    font-weight: normal;
    color: var(--muted);
    margin: 0 0 0.5rem;
}

.backlinks ul {
    list-style: none;
    margin: 0;
    padding: 0;
}

.backlinks li {
    margin-bottom: 0.25rem;
}
```

---

## Pages

### Home Page
- List of 10 most recent posts (all types)
- Pagination link to older posts

### Archive Page (`/archive`)
Three sections:

1. **Favorites** - All posts with `favorite: true`, sorted by date
2. **Tags** - List of all tags with post counts: `#tagname (5)`
3. **Posts by Year** - Chronological list grouped by year

Each list item shows:
- Date (MM-DD format for yearly section)
- Post type icon
- Title (with star if favorite, rating if review)

### Gallery Page (`/gallery`)
- Grid of all photo posts (3 columns, 2 on mobile)
- Square thumbnails with object-fit: cover

### Search Page (`/search`)
- Search input field
- Client-side search using pre-built index
- Results show title and excerpt

### Tag Pages
- List all posts with the specific tag
- Same format as archive list

### Static Pages
- **About** (`/about`)
- **Colophon** (`/colophon`)

---

## Post Type Icons (SVGs)

All icons are 14x14 (archive) or 16x16 (similar posts), stroke-based.

### Regular Post (Book)
```html
<svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
  <path d="M4 19.5v-15A2.5 2.5 0 0 1 6.5 2H20v20H6.5a2.5 2.5 0 0 1 0-5H20"/>
</svg>
```

### Quote
```html
<svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
  <path d="M3 21c3 0 7-1 7-8V5c0-1.25-.756-2.017-2-2H4c-1.25 0-2 .75-2 1.972V11c0 1.25.75 2 2 2 1 0 1 0 1 1v1c0 1-1 2-2 2s-1 .008-1 1.031V21z"/>
  <path d="M15 21c3 0 7-1 7-8V5c0-1.25-.757-2.017-2-2h-4c-1.25 0-2 .75-2 1.972V11c0 1.25.75 2 2 2h.75c0 2.25.25 4-2.75 4v3z"/>
</svg>
```

### Link
```html
<svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
  <path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"/>
  <path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"/>
</svg>
```

### Photo
```html
<svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
  <rect width="18" height="18" x="3" y="3" rx="2" ry="2"/>
  <circle cx="9" cy="9" r="2"/>
  <path d="m21 15-3.086-3.086a2 2 0 0 0-2.828 0L6 21"/>
</svg>
```

### Album (Disc)
```html
<svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
  <circle cx="12" cy="12" r="10"/>
  <circle cx="12" cy="12" r="3"/>
</svg>
```

### Movie (Film)
```html
<svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
  <rect width="20" height="20" x="2" y="2" rx="2.18" ry="2.18"/>
  <line x1="7" x2="7" y1="2" y2="22"/>
  <line x1="17" x2="17" y1="2" y2="22"/>
  <line x1="2" x2="22" y1="12" y2="12"/>
  <line x1="2" x2="7" y1="7" y2="7"/>
  <line x1="2" x2="7" y1="17" y2="17"/>
  <line x1="17" x2="22" y1="17" y2="17"/>
  <line x1="17" x2="22" y1="7" y2="7"/>
</svg>
```

### Book (Open Book)
```html
<svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
  <path d="M2 3h6a4 4 0 0 1 4 4v14a3 3 0 0 0-3-3H2z"/>
  <path d="M22 3h-6a4 4 0 0 0-4 4v14a3 3 0 0 1 3-3h7z"/>
</svg>
```

---

## File Structure

```
/
├── config.toml (or hugo.toml)
├── content/
│   ├── _index.md
│   ├── about.md
│   ├── colophon.md
│   ├── archive.md
│   ├── search.md
│   ├── gallery.md
│   └── blog/
│       ├── _index.md
│       ├── 2024-01-15_post-title.md
│       └── ...
├── static/
│   ├── fonts/
│   │   ├── iowan-old-style-regular.ttf
│   │   ├── iowan-old-style-italic.ttf
│   │   ├── iowan-old-style-bold.ttf
│   │   └── ...
│   └── images/
├── layouts/ (or templates/)
│   ├── _default/
│   │   ├── baseof.html
│   │   ├── list.html
│   │   └── single.html
│   ├── partials/
│   │   ├── post-preview.html
│   │   ├── post-icon.html
│   │   └── similar-posts.html
│   └── ...
└── assets/
    └── scss/
        └── style.scss
```

---

## Responsive Breakpoints

**Mobile (max-width: 600px):**
- Font size: 18px (from 22px)
- Site padding: 2rem 1rem
- Nav wraps with smaller gaps
- Gallery: 2 columns (from 3)
- Similar posts: 1 column (from 3)
- Reviews: Stack vertically

---

## Summary of Frontmatter Fields

| Field | Type | Used By | Description |
|-------|------|---------|-------------|
| `title` | string | All | Post title |
| `date` | date | All | Publication date |
| `tags` | array | All | List of tags |
| `post_type` | string | All | post, quote, link, photo, album, movie, book |
| `favorite` | bool | All | Shows star icon |
| `link_url` | string | link | External URL |
| `photo_url` | string | photo | Image path |
| `artist` | string | album | Artist name |
| `album` | string | album | Album title |
| `author` | string | book | Author name |
| `year` | int | album, movie, book | Release year |
| `rating` | string | album, movie, book | Letter grade (A+ to F) |
| `cover_url` | string | album, book | Cover image path |
| `poster_url` | string | movie | Poster image path |

---

## Implementation Notes for Hugo

1. **Post Types**: Use Hugo's `layouts/blog/single.html` with conditionals based on `.Params.post_type`

2. **Partials**: Create `post-preview.html` partial for list views, similar to Zola's macro

3. **Taxonomies**: Configure tags in `hugo.toml`:
   ```toml
   [taxonomies]
     tag = "tags"
   ```

4. **Search**: Use Pagefind, Lunr.js, or Fuse.js with Hugo's JSON output

5. **RSS**: Built-in with Hugo, configure in `hugo.toml`

6. **SCSS**: Use Hugo Pipes for SCSS compilation

7. **Similar Posts**: Use Hugo's `.Site.RegularPages` and filter by matching tags

8. **Backlinks**: Iterate all pages and check if `.RawContent` contains current page URL
