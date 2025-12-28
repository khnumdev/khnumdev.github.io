# Andrés Pérez Gil - Personal Blog

A personal blog about engineering, big data, and philosophy, powered by Jekyll and hosted on GitHub Pages.

## About

This blog contains personal thoughts and opinions on technology, software engineering, and philosophical topics. Posts cover various aspects of software development, data engineering, and reflections on the impact of technology on society.

## Tech Stack

- **Jekyll** - Static site generator (v3.9.5 via GitHub Pages)
- **GitHub Pages** - Hosting platform with automatic deployment
- **Minima** - Clean and minimal Jekyll theme (v2.5)
- **Kramdown** - Markdown processor
- **Rouge** - Syntax highlighter for code blocks

## Prerequisites

Before you begin, ensure you have the following installed:

- **Ruby** (version 3.1.0 or compatible with GitHub Pages)
- **Bundler** (gem package manager)
- **Git** (for version control)

To check your Ruby version:
```bash
ruby --version
```

To install Bundler:
```bash
gem install bundler
```

## Local Development Setup

Follow these steps to run the site locally:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/khnumdev/khnumdev.github.io.git
   cd khnumdev.github.io
   ```

2. **Install dependencies:**
   ```bash
   bundle install
   ```

3. **Run the local development server:**
   ```bash
   bundle exec jekyll serve
   ```

4. **Access the site:**
   Open your browser and navigate to `http://localhost:4000`

The site will automatically reload when you make changes to most files. Note that changes to `_config.yml` require restarting the server.

## Creating a New Post

### File Naming Convention

Posts must follow this naming pattern in the `_posts/` directory:
```
YYYY-MM-DD-title.markdown
```

For example: `2024-01-15-my-new-post.markdown`

### Required Front Matter

Every post must include front matter at the beginning of the file:

```yaml
---
layout: post
title:  "Your Post Title"
date:   YYYY-MM-DD HH:MM:SS +0200
tags: tag1, tag2
categories: category1, category2
---
```

### Example Post Template

Create a new file in `_posts/` directory:

```markdown
---
layout: post
title:  "My Awesome Blog Post"
date:   2024-01-15 14:30:00 +0200
tags: technology, programming
categories: tech, tutorial
---

Your content starts here. You can use **Markdown** formatting.

## Subheadings

- Bullet points
- Are supported

Code blocks with syntax highlighting:

```python
def hello_world():
    print("Hello, World!")
```

And much more!
```

### Location

All blog posts should be placed in the `_posts/` directory at the root of the repository.

## Project Structure

```
khnumdev.github.io/
├── _posts/              # Blog post markdown files
├── _config.yml          # Site configuration
├── _site/               # Generated site (not committed)
├── .gitignore           # Git ignore rules
├── .ruby-version        # Ruby version specification
├── Gemfile              # Ruby dependencies
├── Gemfile.lock         # Locked dependency versions
├── LICENSE              # Project license
├── README.md            # This file
├── 404.html             # Custom 404 error page
├── links.md             # Links page
└── tutorials.md         # Tutorials page
```

### Key Files

- **`_config.yml`** - Main configuration file for site settings, author info, social links, and plugins
- **`Gemfile`** - Defines Ruby gems and dependencies for the project
- **`_posts/`** - Contains all blog post markdown files
- **`404.html`** - Custom error page for broken links
- **`links.md`** & **`tutorials.md`** - Additional site pages

## Configuration

The site is configured through `_config.yml`. Key settings include:

- **Site metadata**: Title, description, email, social usernames
- **Timezone**: `Europe/Madrid` (GMT+2)
- **Pagination**: 5 posts per page
- **Theme**: Minima
- **Plugins**: Jekyll Feed, Jekyll Paginate, Jekyll SEO Tag

To modify site settings:

1. Open `_config.yml`
2. Edit the desired values
3. Restart the Jekyll server (changes to config require a restart)

### Available Configuration Options

- `title` - Site title displayed in header
- `email` - Contact email address
- `description` - Site description for SEO
- `timezone` - Site timezone (default: Europe/Madrid)
- `paginate` - Number of posts per page (default: 5)
- `author` - Author information for SEO
- `social` - Social media links for SEO

## Deployment

This site is automatically deployed via **GitHub Pages** when changes are pushed to the `main` branch.

### Deployment Process

1. Make your changes locally
2. Test with `bundle exec jekyll serve`
3. Commit your changes:
   ```bash
   git add .
   git commit -m "Your commit message"
   ```
4. Push to GitHub:
   ```bash
   git push origin main
   ```
5. GitHub Pages will automatically build and deploy your site within a few minutes

The site is available at: `https://khnumdev.github.io/`

### Build Status

Check the Actions tab in the GitHub repository to monitor deployment status and view any build errors.

## Theme Customization

This site uses the **Minima** theme (v2.5). 

### Theme Documentation

- [Minima GitHub Repository](https://github.com/jekyll/minima)
- [Minima Customization Guide](https://github.com/jekyll/minima#customization)

### Overriding Theme Defaults

To customize the theme:

1. **Override layouts**: Create files in `_layouts/` directory
2. **Override includes**: Create files in `_includes/` directory
3. **Custom CSS**: Create `assets/main.scss` and import the theme
4. **Custom pages**: Add `.md` or `.html` files to the root directory

Example custom CSS file (`assets/main.scss`):

```scss
---
---

@import "minima";

// Your custom CSS here
```

## License

This project is open source and available under the terms specified in the [LICENSE](LICENSE) file.

## Contact

- **Author**: Andrés Pérez Gil
- **Email**: helloandres@outlook.com
- **Twitter**: [@AndresPerezGil](https://twitter.com/AndresPerezGil)
- **GitHub**: [@khnumdev](https://github.com/khnumdev)
- **LinkedIn**: [andresperezgil](https://www.linkedin.com/in/andresperezgil)

---

Built with ❤️ using Jekyll and GitHub Pages
