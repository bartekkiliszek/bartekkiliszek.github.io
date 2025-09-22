---
layout: home
title: Home
---

# Welcome to My Blog

Hi, I'm Bartek Kiliszek, a backend developer passionate about Java and software engineering. This is where I share my thoughts, experiences, and insights from my journey in the world of programming.

## Recent Posts

{% for post in site.posts limit:5 %}
### [{{ post.title }}]({{ post.url | relative_url }})
*{{ post.date | date: "%B %d, %Y" }}*

{{ post.excerpt }}

[Read more →]({{ post.url | relative_url }})

---
{% endfor %}

## What You'll Find Here

- **Backend Development**: Deep dives into server-side technologies, APIs, and system design
- **Java Journey**: My learning experiences, tips, and tricks with Java
- **Software Engineering**: Best practices, patterns, and architectural insights
- **Code Snippets**: Practical examples and solutions to common problems

## Connect With Me

- Twitter: [@bartekkiliszek](https://twitter.com/bartekkiliszek)
- GitHub: [@bartekkiliszek](https://github.com/bartekkiliszek)

[View all posts →](/archive)