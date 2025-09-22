# Bartek Kiliszek's Blog

Welcome to my personal blog repository! This is where I share my journey as a backend developer, insights about Java, and various aspects of software engineering.

## About This Blog

This blog is built with Jekyll and hosted on GitHub Pages. It focuses on:

- **Backend Development**: Server-side technologies, APIs, and system design
- **Java Programming**: Deep dives into Java, frameworks, and best practices
- **Software Engineering**: Design patterns, architecture, and development workflows
- **Learning Journey**: My experiences and insights as I grow as a developer

## Quick Start

To run this blog locally:

1. Clone the repository:
   ```bash
   git clone https://github.com/bartekkiliszek/bartekkiliszek.github.io.git
   cd bartekkiliszek.github.io
   ```

2. Install Jekyll and dependencies:
   ```bash
   gem install bundler jekyll
   bundle install
   ```

3. Run the development server:
   ```bash
   bundle exec jekyll serve
   ```

4. Open your browser to `http://localhost:4000`

## Blog Structure

```
├── _posts/                 # Blog posts (YYYY-MM-DD-title.md)
├── _layouts/               # Page templates
├── assets/css/             # Stylesheets
├── _config.yml             # Jekyll configuration
├── index.md                # Homepage
├── about.md                # About page
└── archive.md              # Blog archive
```

## Writing Posts

Create new blog posts in the `_posts` directory with the naming convention:
`YYYY-MM-DD-title.md`

### Front Matter Template

```yaml
---
layout: post
title: "Your Post Title"
date: YYYY-MM-DD
categories: [Category1, Category2]
tags: [tag1, tag2, tag3]
excerpt: "Brief description of your post"
---
```

## Features

- Clean, responsive design
- Syntax highlighting for code blocks
- Dark mode support
- SEO optimization
- RSS feed
- Post categories and tags
- Social sharing
- Navigation between posts

## About Me

👋 Hi, I'm Bartek Kiliszek
👀 I'm passionate about backend development
🌱 Currently diving deep into Java and its ecosystem
📫 Connect with me on Twitter [@bartekkiliszek](https://twitter.com/bartekkiliszek)

## License

This blog content is available under the [MIT License](LICENSE). Feel free to use the code structure for your own blog!
