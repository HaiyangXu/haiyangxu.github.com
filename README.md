# Haiyang Xu's Personal Blog

This is a Jekyll-based blog hosted on GitHub Pages, featuring posts about computer vision, algorithms, and technology.

## 🔧 Setup & Configuration

This site has been updated to work with modern GitHub Pages (2025):

### Key Updates Made:
- ✅ Updated Jekyll configuration for modern GitHub Pages
- ✅ Added proper `Gemfile` with GitHub Pages gem
- ✅ Fixed deprecated `pygments` → `highlighter: rouge`
- ✅ Updated analytics and comments configurations
- ✅ Added GitHub Actions workflow for automated builds
- ✅ Improved markdown processing with kramdown + GFM

### Local Development

1. **Install Ruby and Bundler** (if not already installed):
   ```bash
   # On Windows, install Ruby via RubyInstaller
   # Then install bundler
   gem install bundler
   ```

2. **Install dependencies**:
   ```bash
   bundle install
   ```

3. **Run locally**:
   ```bash
   bundle exec jekyll serve
   ```
   
   The site will be available at `http://localhost:4000`

### Deployment

The site automatically deploys via GitHub Actions when you push to the main branch. Make sure:

1. Repository settings → Pages → Source is set to "GitHub Actions"
2. The repository is public (or you have GitHub Pro/Team for private repo Pages)

### Configuration Notes

- **Analytics**: Update `_config.yml` with your Google Analytics 4 measurement ID
- **Comments**: Uses Disqus - update with your Disqus shortname in `_config.yml`
- **Social**: Update author information in `_config.yml`

### Theme

This site uses the Jekyll Bootstrap framework with the Mark Reid theme.

## 📝 Writing Posts

Create new posts in the `_posts` directory with the naming convention:
```
YYYY-MM-DD-title-of-post.md
```

Example front matter:
```yaml
---
layout: post
title: "Your Post Title"
description: "Brief description"
category: "category-name"
tags: ["tag1", "tag2"]
---
```

## 🚀 Features

- Responsive design
- Syntax highlighting with Rouge
- SEO optimization with jekyll-seo-tag
- RSS feed generation
- Sitemap generation
- Archive and category pages

---

**Blog URL**: https://haiyangxu.github.io  
**Last Updated**: June 2025 - Modernized for current GitHub Pages standards
