# Deteritus

A minimalist personal blog built with Hugo. Features multiple post types, reviews with ratings, a favorites system, and clean typography-focused design.

## Quick Start

```bash
# Install Hugo (if not installed)
brew install hugo

# Serve locally with live reload
hugo server -D

# Build for production
hugo
```

The site will be available at `http://localhost:1313/`.

## Creating Posts

All posts go in the `content/blog/` directory.

### File Naming Convention

```
YYYY-MM-DD_slug-name.md
```

Example: `2024-02-25_on-reading.md`

### Basic Post Structure

Every post needs frontmatter at the top:

```yaml
---
slug: "your-slug"        # Required: determines the URL (e.g., /your-slug/)
title: "Post Title"
date: 2024-02-25
tags: ["tag1", "tag2"]
favorite: true           # Optional: adds star icon
---

Your content here...
```

**Important:** The `slug` field determines the URL. A post with `slug: "on-reading"` will be available at `/on-reading/`.

## Post Types

### 1. Regular Post (default)

Standard blog post with title and content.

```yaml
---
slug: "my-thoughts"
title: "My Thoughts on X"
date: 2024-02-25
tags: ["thoughts"]
favorite: true  # Optional
---
```

### 2. Quote Post

For sharing quotes without a title. Displays with green left border and italic text.

```yaml
---
slug: "sagan-quote"
title: "Carl Sagan on Reading"  # Used for archive/search
date: 2024-01-20
tags: ["quotes"]
post_type: "quote"
attribution: "Carl Sagan"  # Optional: displayed separately with different styling
---

The quote text here...
```

### 3. Link Post

For sharing external links with commentary. Title links to external URL with arrow.

```yaml
---
slug: "interesting-article"
title: "The Web's Grain"
date: 2024-01-25
tags: ["links", "web"]
post_type: "link"
link_url: "https://example.com/article"
favorite: true  # Optional
---

Your commentary on the linked article...
```

### 4. Photo Post

For sharing photos with captions.

```yaml
---
slug: "morning-light"
title: "Morning Light"
date: 2024-02-01
tags: ["photos", "nature"]
post_type: "photo"
photo_url: "/images/morning-light.jpg"
---

Caption and description...
```

### 5. Album Review

For music album reviews with ratings. Cover image appears on the right with a caption.

```yaml
---
slug: "ok-computer-review"
title: "OK Computer"
date: 2024-02-10
tags: ["music", "reviews"]
post_type: "album"
artist: "Radiohead"
album: "OK Computer"
year: 1997
rating: "A+"
cover_url: "/images/ok-computer.jpg"  # Optional
cover_caption: "Custom caption here"  # Optional - defaults to "Album (Year)"
favorite: true  # Optional
---

Your review...
```

### 6. Movie Review

For film reviews with ratings. Poster appears on the right with a caption.

```yaml
---
slug: "parasite-review"
title: "Parasite"
date: 2024-02-15
tags: ["film", "reviews"]
post_type: "movie"
year: 2019
rating: "A"
poster_url: "/images/parasite.jpg"  # Optional
cover_caption: "Custom caption here"  # Optional - defaults to "Title (Year)"
favorite: true  # Optional
---

Your review...
```

### 7. Book Review

For book reviews with ratings. Cover appears on the right with a caption.

```yaml
---
slug: "slaughterhouse-five-review"
title: "Slaughterhouse-Five"
date: 2024-02-20
tags: ["books", "reviews"]
post_type: "book"
author: "Kurt Vonnegut"
year: 1969
rating: "A+"
cover_url: "/images/slaughterhouse-five.jpg"  # Optional
cover_caption: "Custom caption here"  # Optional - defaults to "Author, Year"
favorite: true  # Optional
---

Your review...
```

## Rating System

Reviews use letter grades: **A+, A, A-, B+, B, B-, C+, C, C-, D+, D, D-, F**

Ratings appear as color-coded badges next to the title:
- **A+, A**: Green (excellent)
- **A-, B+**: Light green (very good)
- **B, B-**: Olive (good)
- **C+, C, C-**: Gray (average)
- **D+, D, D-**: Tan (below average)
- **F**: Dusty rose (poor)

## Favorites

Add `favorite: true` to any post's frontmatter to mark it as a favorite. A star icon will appear next to the title, and the post will be listed in the Favorites section of the Archive page.

## Images

Place images in `static/images/`. Reference them in frontmatter as `/images/filename.jpg`.

## Site Structure

- **Home** (`/`): Latest 10 posts with pagination
- **Archive** (`/archive/`): All posts organized by Favorites, Tags, post type (Movies, Albums, Books), and chronologically
- **Gallery** (`/gallery/`): Grid of all photo posts
- **Search** (`/search/`): Client-side search powered by Pagefind
- **About** (`/about/`): About page
- **Colophon** (`/colophon/`): Site credits and technical details
- **Tags** (`/tags/tagname/`): Posts filtered by tag, grouped by year

## Features

- **Clean URLs**: Posts are accessible at `/:slug/` (e.g., `/on-reading/`)
- **Dark mode**: Automatically follows system preference
- **Similar posts**: Shows related posts based on shared tags
- **Backlinks**: Shows which posts link to the current post
- **RSS feed**: Available at `/index.xml`
- **Responsive design**: Optimized for mobile and desktop

## Theme Customization

The theme files are in `themes/deteritus/`:

- `layouts/`: HTML templates
- `static/css/style.css`: All styles with CSS custom properties
- `static/fonts/`: Iowan Old Style font files

### CSS Variables

Edit colors in `static/css/style.css`:

```css
:root {
  --bg: #fffff8;      /* Background */
  --text: #111;       /* Text color */
  --muted: #666;      /* Secondary text */
  --link: #2a7a4b;    /* Link color (forest green) */
  --border: #ddd;     /* Border color */
}
```

## Rebuilding Search Index

After adding new posts locally, rebuild the Pagefind index:

```bash
hugo && npx pagefind --site public
```

## Deployment

The site automatically deploys to GitHub Pages when you push to the `main` branch.

### Initial Setup

1. **Create GitHub repository** (if not already done):
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git remote add origin git@github.com:tiagojct/deteritus.com.git
   git push -u origin main
   ```

2. **Enable GitHub Pages**:
   - Go to repository Settings → Pages
   - Under "Build and deployment", select **GitHub Actions**

3. **Configure custom domain** (optional):
   - In Settings → Pages, add your custom domain
   - Configure DNS with your domain provider:
     - For apex domain (deteritus.com): Add A records pointing to GitHub's IPs
     - For www subdomain: Add CNAME record pointing to `tiagojct.github.io`

### Manual Deployment

To deploy manually:
```bash
git add .
git commit -m "Your commit message"
git push
```

The GitHub Action will automatically build Hugo, generate the search index, and deploy.

## Creating Posts

Use the `newpost` script to create posts from archetypes:

```bash
./newpost <type> <slug>
```

### Available Types

| Type | Use For |
|------|---------|
| `post` | Regular blog posts |
| `quote` | Quote posts with attribution |
| `link` | External link posts |
| `photo` | Photo posts |
| `album` | Music album reviews |
| `movie` | Film reviews |
| `book` | Book reviews |

### Examples

```bash
./newpost post my-thoughts
./newpost quote sagan-on-books
./newpost link interesting-article
./newpost album ok-computer-review
./newpost movie parasite-review
./newpost book slaughterhouse-five
```

This creates a file like `content/blog/2024-01-15_my-thoughts.md` with the appropriate frontmatter template.

## Frontmatter Reference

| Field | Type | Used By | Description |
|-------|------|---------|-------------|
| `slug` | string | All | URL path (required) |
| `title` | string | All | Post title |
| `date` | date | All | Publication date |
| `tags` | array | All | List of tags |
| `post_type` | string | All | post, quote, link, photo, album, movie, book |
| `favorite` | bool | All | Shows star icon |
| `attribution` | string | quote | Quote attribution (author, source) |
| `link_url` | string | link | External URL |
| `photo_url` | string | photo | Image path |
| `artist` | string | album | Artist name |
| `album` | string | album | Album title |
| `author` | string | book | Author name |
| `year` | int | album, movie, book | Release year |
| `rating` | string | album, movie, book | Letter grade (A+ to F) |
| `cover_url` | string | album, book | Cover image path |
| `poster_url` | string | movie | Poster image path |
| `cover_caption` | string | album, movie, book | Custom caption for cover/poster image |
