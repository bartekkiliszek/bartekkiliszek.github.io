# Content Creation Workflow

This document outlines the collaborative, iterative process for creating blog posts. The workflow is designed to be flexible - adapt it to your needs!

## Overview

The content pipeline consists of four main stages:

```
Ideation → Outline → Draft (iterate) → Review → Publish
```

**Key Principle**: Iterate as much as needed at each stage. Quality over speed!

## Folder Structure

```
_content_pipeline/
  outlines/              # Stage 1: Initial ideas and outlines

_drafts/                 # Stage 2-3: Working drafts (Jekyll built-in)
                         # Preview with: jekyll serve --drafts

_templates/              # Reusable templates
  1-outline-template.md
  2-draft-template.md
  3-review-checklist.md

_posts/                  # Stage 4: Published posts
```

## Stage 1: Ideation & Outline

**Goal**: Capture and structure your idea before writing

### Process

1. **Copy the outline template**:
   ```bash
   cp _templates/1-outline-template.md _content_pipeline/outlines/my-post-idea.md
   ```

2. **Fill in what you know**:
   - You don't need to complete every section
   - Focus on: topic, audience, key points
   - Rough ideas are fine!

3. **Collaborate with Claude**:
   - Share your outline
   - Ask for feedback on structure
   - Discuss approach and depth
   - Refine key points together

4. **Iterate until clear**:
   - The outline should give you a roadmap
   - You should know what the post will cover
   - Structure should feel logical

### Example Prompts for Claude

- "I've filled out an outline in `_content_pipeline/outlines/spring-boot-intro.md`. Can you review it and suggest improvements?"
- "Help me expand point #3 in my outline - I'm not sure how to structure it"
- "Does this outline make sense for a beginner audience?"
- "What code examples would work well for this topic?"

## Stage 2: First Draft

**Goal**: Transform outline into a complete post

### Process

1. **Copy the draft template**:
   ```bash
   cp _templates/2-draft-template.md _drafts/spring-boot-intro.md
   ```

2. **Fill in front matter**:
   - Title, categories, tags, excerpt
   - Date (use intended publication date)
   - Layout should stay as `post`

3. **Write or collaborate**:

   **Option A - You Write First**:
   - Use outline as your guide
   - Fill in sections at your own pace
   - Leave comments where you're unsure
   - Share with Claude for feedback

   **Option B - Collaborate from Start**:
   - Share outline with Claude
   - Work section by section together
   - Claude can draft, you review and edit
   - Iterate on each section

4. **Preview locally**:
   ```bash
   bundle exec jekyll serve --drafts
   ```
   - View at `http://localhost:4000`
   - Check formatting, code highlighting
   - See how it looks on your site

### Example Prompts for Claude

- "Here's my outline. Can you help me draft the introduction?"
- "I've written the first two sections in `_drafts/my-post.md`. Can you review them?"
- "Help me write a code example for [specific concept]"
- "This section feels too technical - can you help make it more accessible?"
- "Can you draft section 3 based on the outline? I'll review and edit."

## Stage 3: Iteration & Refinement

**Goal**: Polish the draft through multiple rounds of review

### Process

This is the heart of the iterative workflow. Expect 2-5 rounds of refinement:

1. **Review yourself**:
   - Read through the entire post
   - Mark sections that need work
   - Note questions or concerns

2. **Get Claude's feedback**:
   - Ask for specific types of feedback
   - Technical accuracy check
   - Clarity and flow
   - Code examples review

3. **Make improvements**:
   - Address feedback
   - Refine unclear sections
   - Add examples where needed
   - Improve transitions

4. **Repeat as needed**:
   - Keep iterating until satisfied
   - No rush - quality matters!
   - Test all code examples

### Example Prompts for Claude

- "Review `_drafts/my-post.md` for technical accuracy"
- "Does the flow make sense? Any sections feel disconnected?"
- "The code example in section 2 feels incomplete - can you expand it?"
- "Help me improve the conclusion - it feels weak"
- "Can you suggest better transitions between sections?"
- "Read through and point out anything confusing or unclear"

### Tips for Effective Iteration

- **Be specific**: Tell Claude what kind of feedback you want
- **One section at a time**: Don't try to perfect everything at once
- **Test code**: Run examples as you go, not just at the end
- **Read aloud**: Helps catch awkward phrasing
- **Take breaks**: Fresh eyes catch more issues
- **Preview often**: See changes in context on your site

## Stage 4: Final Review & Publish

**Goal**: Final quality check and publication

### Process

1. **Use the review checklist**:
   ```bash
   cat _templates/3-review-checklist.md
   ```
   - Go through each relevant item
   - Don't skip the technical checks!
   - Verify all code examples one more time

2. **Final preview**:
   - View post locally with `jekyll serve --drafts`
   - Check on desktop and mobile (if possible)
   - Click all links to verify
   - Read through one final time

3. **Prepare for publication**:
   - Ensure front matter is complete
   - Date is correct
   - Filename follows convention

4. **Publish**:
   ```bash
   # Move from drafts to posts with proper filename
   mv _drafts/my-post.md _posts/2025-01-25-spring-boot-introduction.md

   # Commit and push
   git add _posts/2025-01-25-spring-boot-introduction.md
   git commit -m "Add post: Spring Boot Introduction"
   git push origin main
   ```

5. **Post-publish**:
   - Verify live on GitHub Pages (may take a few minutes)
   - Check formatting looks good
   - Test links on live site
   - Share on social media if desired!

## Working with Claude: Best Practices

### Getting the Most Out of Collaboration

**Do:**
- Be specific about what you need help with
- Share context about your audience and goals
- Ask for explanations, not just answers
- Request multiple options or approaches
- Iterate on feedback rather than accepting first draft
- Ask "why" when you don't understand suggestions

**Don't:**
- Expect perfection on first try (that's why we iterate!)
- Accept technical content without understanding it
- Skip testing code that Claude provides
- Hesitate to disagree or ask for alternatives
- Feel obligated to use every suggestion

### Example Collaboration Patterns

**Pattern 1: You Outline, Claude Drafts**
```
1. You: Fill out outline template
2. Claude: Review and refine outline
3. You: Approve structure
4. Claude: Draft sections based on outline
5. You: Review, edit, provide feedback
6. Claude: Refine based on feedback
7. Repeat steps 5-6 until satisfied
```

**Pattern 2: Parallel Creation**
```
1. You: Write some sections
2. Claude: Write other sections
3. Together: Review and unify tone/style
4. Together: Fill gaps and smooth transitions
5. Claude: Final polish pass
```

**Pattern 3: Iterative Co-creation**
```
1. Together: Brainstorm in outline
2. Together: Draft section 1
3. Together: Review section 1, iterate
4. Together: Move to section 2
5. Repeat for each section
6. Together: Polish as whole
```

## Tips for Success

### Writing Tips

- **Start with what you know**: Write easy sections first to build momentum
- **Explain like you're teaching**: Imagine explaining to a friend
- **Use examples liberally**: Code, analogies, real-world scenarios
- **Break up text**: Short paragraphs, bullet points, code blocks
- **Edit ruthlessly**: First draft is never the final draft

### Collaboration Tips

- **Communicate goals**: Tell Claude what you're trying to achieve
- **Ask questions**: Better to ask than to publish confusion
- **Request alternatives**: "Can you show me another way to explain this?"
- **Build iteratively**: Perfect one section before moving to next
- **Trust the process**: Good posts take time and iteration

### Technical Tips

- **Test code early**: Don't wait until the end
- **Use correct syntax highlighting**: Specify language in code blocks
- **Preview often**: Catch formatting issues as you go
- **Keep it simple**: Complex examples confuse readers
- **Provide context**: Include imports, setup, etc.

## Version Control & Git

### What to Track

**Definitely commit**:
- Published posts in `_posts/`
- Templates in `_templates/`
- This workflow document
- Site configuration and layouts

**Optional** (your choice):
- Outlines in `_content_pipeline/outlines/`
- Drafts in `_drafts/`

### Suggested .gitignore Additions

If you want to keep work-in-progress private:

```
# Content pipeline (work in progress)
_content_pipeline/
_drafts/

# Or keep outlines but not drafts:
# _drafts/
```

### Publishing Commits

```bash
# Add specific post
git add _posts/2025-01-25-my-new-post.md
git commit -m "Add post: My New Post Title"

# Or add all changes
git add .
git commit -m "Add post: My New Post Title"

# Push to GitHub (triggers GitHub Pages rebuild)
git push origin main
```

## Troubleshooting

### Post Not Showing Up

- **Check date**: Posts dated in future won't show (use past or today's date)
- **Check filename**: Must be `YYYY-MM-DD-title.md` format
- **Check location**: Must be in `_posts/` not `_drafts/`
- **Check front matter**: Must have proper YAML front matter
- **Wait**: GitHub Pages can take 1-2 minutes to rebuild

### Code Not Highlighting

- **Specify language**: Use ```java not just ```
- **Check syntax**: Ensure code is valid
- **Preview locally**: Test with `jekyll serve`

### Formatting Issues

- **Check markdown**: Use proper markdown syntax
- **Check indentation**: Especially in lists and code blocks
- **Preview locally**: See issues before publishing
- **Validate YAML**: Front matter must be valid YAML

## Quick Reference

### Common Commands

```bash
# Preview site with drafts
bundle exec jekyll serve --drafts

# Preview site without drafts
bundle exec jekyll serve

# Create new post from template
cp _templates/2-draft-template.md _drafts/my-new-post.md

# Publish a draft
mv _drafts/my-post.md _posts/2025-01-25-my-post-title.md

# View templates
ls _templates/
```

### File Naming Convention

```
Outlines:  descriptive-name.md
Drafts:    descriptive-name.md
Published: YYYY-MM-DD-title-slug.md
```

### Front Matter Template

```yaml
---
layout: post
title: "Your Title Here"
date: 2025-01-25
categories: [Java, Backend]
tags: [spring-boot, tutorial, beginner]
excerpt: "Brief description for listings and SEO"
---
```

## Customizing This Workflow

This workflow is a starting point. Feel free to adapt it:

- **Skip stages**: Quick posts don't need extensive outlines
- **Add stages**: Add research or peer review steps
- **Change templates**: Modify templates to fit your style
- **Simplify**: Use fewer templates if it feels like overhead
- **Expand**: Add more structure if helpful

The goal is to help you create quality content efficiently, not to create bureaucracy!

---

## Questions?

When working with Claude on posts:
- Reference this workflow document
- Be specific about which stage you're in
- Ask for help with any part of the process

Happy writing!
