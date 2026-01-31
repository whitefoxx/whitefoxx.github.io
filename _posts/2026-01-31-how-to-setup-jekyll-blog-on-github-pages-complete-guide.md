---
layout: post
title: 'How to Set Up a Jekyll Blog on GitHub Pages: Complete Guide'
date: 2026-01-31 00:00:00 +0800
categories: [Tutorial, Jekyll, GitHub Pages]
tags: [Jekyll, GitHub Pages, Blogging, Web Development]
excerpt: 'A comprehensive step-by-step guide to setting up your own Jekyll blog on GitHub Pages, including how to add new posts and best practices.'
---

# How to Set Up a Jekyll Blog on GitHub Pages: Complete Guide

Welcome to this complete guide on setting up your own blog using Jekyll and GitHub Pages! This tutorial will walk you through everything you need to know, from initial setup to adding new content.

## What is Jekyll?

[Jekyll](https://jekyllrb.com/) is a static site generator that transforms plain text into static websites and blogs. It's perfect for:

- Personal blogs
- Project documentation
- Portfolio sites
- Simple company websites

**Why use Jekyll?**

- ✅ Free and open source
- ✅ No database required
- ✅ Fast and secure
- ✅ Easy to deploy on GitHub Pages
- ✅ Write posts in Markdown
- ✅ Built-in blogging features

## What is GitHub Pages?

[GitHub Pages](https://pages.github.com/) is a static site hosting service that takes HTML, CSS, and JavaScript files straight from a repository on GitHub, optionally runs the files through a build process, and publishes a website.

**Key benefits:**

- ✅ Free hosting
- ✅ Automatic HTTPS
- ✅ Custom domain support
- ✅ Automatic Jekyll builds
- ✅ Git-based version control

## 📁 Project Structure

A typical Jekyll blog structure looks like this:

```
your-username.github.io/
├── _config.yml          # Jekyll configuration file
├── Gemfile              # Ruby dependencies
├── index.md             # Homepage (lists all posts)
├── about.md             # About page
├── _layouts/            # HTML templates
│   ├── default.html     # Base layout
│   └── post.html        # Blog post layout
└── _posts/              # Your blog posts (YYYY-MM-DD-title.md)
```

## 🚀 Step-by-Step Setup

### Step 1: Create GitHub Repository

1. Go to [GitHub](https://github.com/) and sign in
2. Click the **+** icon in the top-right corner
3. Select **New repository**
4. Name it `your-username.github.io` (replace with your username)
5. Make it **Public**
6. Click **Create repository**

> **Important:** The repository name must match this exact format for GitHub Pages to work automatically!

### Step 2: Clone the Repository

Open your terminal and run:

```bash
git clone https://github.com/your-username/your-username.github.io.git
cd your-username.github.io
```

### Step 3: Create Configuration File

Create a file named `_config.yml` in the root directory:

```yaml
# Site settings
title: My Blog
description: A personal blog about technology and development
author: Your Name
email: your-email@example.com

# URL settings
baseurl: ''
url: 'https://your-username.github.io'

# Social settings (optional)
twitter_username: your_twitter
github_username: your_username

# Build settings
markdown: kramdown
theme: minima
highlighter: rouge

# Plugins
plugins:
  - jekyll-seo-tag
  - jekyll-sitemap

# Pagination
paginate: 5
paginate_path: '/page/:num/'

# Exclude from processing
exclude:
  - Gemfile
  - Gemfile.lock
  - node_modules
  - vendor/bundle/
  - vendor/cache/
  - vendor/gems/
  - vendor/ruby/

# Front matter defaults
defaults:
  - scope:
      path: ''
      type: 'posts'
    values:
      layout: 'post'
      author: 'Your Name'
```

### Step 4: Create Gemfile

Create a file named `Gemfile`:

```ruby
source "https://rubygems.org"

gem "jekyll", "~> 4.3.3"
gem "github-pages", group: :jekyll_plugins

group :jekyll_plugins do
  gem "jekyll-seo-tag", "~> 2.8.0"
  gem "jekyll-sitemap", "~> 1.4.0"
end

platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.1", platforms: [:mingw, :x64_mingw, :mswin]
gem "http_parser.rb", "~> 0.6.0"
```

### Step 5: Create Layouts

Create a `_layouts` directory and add two files:

#### `default.html` (Base Layout)

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>
      {% if page.title %}{{ page.title }} | {% endif %}{{ site.title }}
    </title>
    <meta
      name="description"
      content="{% if page.excerpt %}{{ page.excerpt | strip_html | strip_newlines | truncate: 160 }}{% else %}{{ site.description }}{% endif %}"
    />
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css"
    />
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"
    />
    <style>
      /* Add your custom styles here */
      body {
        font-family:
          -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
        background-color: #f8fafc;
        color: #1e293b;
        line-height: 1.7;
      }
      .navbar {
        background-color: white;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
      }
      .post-card {
        background: white;
        border-radius: 12px;
        padding: 30px;
        margin-bottom: 30px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
      }
    </style>
  </head>
  <body>
    <nav class="navbar navbar-expand-lg navbar-light sticky-top">
      <div class="container">
        <a class="navbar-brand" href="{{ site.baseurl }}/">{{ site.title }}</a>
        <div class="collapse navbar-collapse">
          <ul class="navbar-nav ms-auto">
            <li class="nav-item">
              <a class="nav-link" href="{{ site.baseurl }}/">Home</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="{{ site.baseurl }}/about">About</a>
            </li>
          </ul>
        </div>
      </div>
    </nav>

    <div class="container" style="padding: 40px 0;">{{ content }}</div>

    <footer
      style="background: white; padding: 40px 0; margin-top: 60px; border-top: 1px solid #e2e8f0;"
    >
      <div class="container text-center">
        <p>
          &copy; {{ site.time | date: "%Y" }} {{ site.author }}. All rights
          reserved.
        </p>
      </div>
    </footer>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
  </body>
</html>
```

#### `post.html` (Post Layout)

```html
---
layout: default
---

<div class="row">
  <div class="col-lg-8">
    <article
      style="background: white; border-radius: 12px; padding: 40px; box-shadow: 0 2px 8px rgba(0,0,0,0.08);"
    >
      <header>
        <h1>{{ page.title }}</h1>
        <div style="color: #64748b; font-size: 0.9rem; margin: 15px 0;">
          <i class="far fa-calendar-alt"></i> {{ page.date | date: "%B %d, %Y"
          }} {% if page.author %}
          <span style="margin: 0 10px;">|</span>
          <i class="far fa-user"></i> {{ page.author }} {% endif %}
        </div>
      </header>

      <div class="mt-4">{{ content }}</div>
    </article>
  </div>

  <div class="col-lg-4">
    <aside>
      <div
        style="background: white; border-radius: 12px; padding: 25px; box-shadow: 0 2px 8px rgba(0,0,0,0.08); margin-bottom: 30px;"
      >
        <h4 style="font-weight: 700; color: #2563eb; margin-bottom: 15px;">
          Recent Posts
        </h4>
        <ul style="list-style: none; padding: 0;">
          {% for post in site.posts limit: 5 %}
          <li style="margin-bottom: 10px;">
            <a
              href="{{ site.baseurl }}{{ post.url }}"
              style="color: #1e293b; text-decoration: none;"
            >
              {{ post.title }}
            </a>
          </li>
          {% endfor %}
        </ul>
      </div>
    </aside>
  </div>
</div>
```

### Step 6: Create Homepage

Create `index.md`:

```markdown
---
layout: default
title: Home
---

<h2>Latest Posts</h2>

{% for post in site.posts %}

<article class="post-card">
  <h3>
    <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
  </h3>
  <div style="color: #64748b; font-size: 0.9rem; margin: 15px 0;">
    <i class="far fa-calendar-alt"></i> {{ post.date | date: "%B %d, %Y" }}
  </div>
  <div>
    {% if post.excerpt %}
      {{ post.excerpt | strip_html | truncatewords: 50 }}
    {% else %}
      {{ post.content | strip_html | truncatewords: 50 }}
    {% endif %}
  </div>
  <div style="margin-top: 20px;">
    <a href="{{ site.baseurl }}{{ post.url }}" style="color: #2563eb; font-weight: 600; text-decoration: none;">
      Read More <i class="fas fa-arrow-right"></i>
    </a>
  </div>
</article>
{% endfor %}
```

### Step 7: Create About Page

Create `about.md`:

```markdown
---
layout: default
title: About
---

<div style="background: white; border-radius: 12px; padding: 40px; box-shadow: 0 2px 8px rgba(0,0,0,0.08);">
  <h1>About This Blog</h1>
  <p>Welcome to my personal blog! This is a space where I share my thoughts, experiences, and insights about technology, software development, and everything in between.</p>
  
  <h2>About Me</h2>
  <p>I'm a passionate developer who loves exploring new technologies and sharing knowledge with the community. This blog serves as my platform to document what I learn and help others along the way.</p>
  
  <h2>Get In Touch</h2>
  <p>I'd love to hear from you! Feel free to reach out through:</p>
  <ul>
    <li><a href="https://github.com/{{ site.github_username }}" target="_blank">GitHub</a></li>
    {% if site.twitter_username %}
    <li><a href="https://twitter.com/{{ site.twitter_username }}" target="_blank">Twitter</a></li>
    {% endif %}
  </ul>
</div>
```

### Step 8: Create Your First Blog Post

Create a `_posts` directory and add your first post:

```markdown
---
layout: post
title: 'My First Blog Post'
date: 2026-01-31 00:00:00 +0800
categories: [Tutorial]
tags: [Jekyll, Blogging]
excerpt: 'Welcome to my first blog post!'
---

# My First Blog Post

Welcome to my new blog! This is my first post, and I'm excited to share my journey with you.

## What You'll Find Here

- Tutorials on various programming topics
- Deep dives into specific technologies
- Personal projects and their development journey
- Best practices and tips for developers

## Getting Started

I've set up this blog using Jekyll and GitHub Pages, which makes it easy to write and publish content.

Stay tuned for more posts!
```

### Step 9: Push to GitHub

```bash
git add .
git commit -m "Initial Jekyll blog setup"
git push origin main
```

### Step 10: Enable GitHub Pages

1. Go to your repository on GitHub
2. Click **Settings** tab
3. Click **Pages** in the left sidebar
4. Under **Source**, select **Deploy from a branch**
5. Select `main` branch and `/ (root)` folder
6. Click **Save**

Your site will be available at `https://your-username.github.io` within 1-2 minutes!

## ✍️ How to Add New Blog Posts

### Step 1: Create a New Post File

Navigate to the `_posts/` directory and create a new file with this naming convention:

```
YYYY-MM-DD-your-post-title.md
```

**Example:** `2026-02-15-how-to-learn-javascript.md`

**Important rules:**

- Use the date you want to publish (YYYY-MM-DD format)
- Use lowercase letters, numbers, and hyphens only
- No spaces or special characters in the filename
- The filename determines the URL structure

### Step 2: Add Front Matter

Every blog post must start with **front matter** - YAML metadata enclosed by three dashes:

```yaml
---
layout: post
title: 'Your Post Title Here'
date: 2026-02-15 00:00:00 +0800
categories: [Tutorial, JavaScript]
tags: [Jekyll, GitHub Pages, Blogging]
excerpt: 'A brief description that appears in post previews'
---
```

**Front Matter Fields Explained:**

| Field        | Required | Description             | Example                     |
| ------------ | -------- | ----------------------- | --------------------------- |
| `layout`     | ✅ Yes   | Which layout to use     | `post`                      |
| `title`      | ✅ Yes   | The post title          | `"How to Learn JavaScript"` |
| `date`       | ✅ Yes   | Publication date        | `2026-02-15 00:00:00 +0800` |
| `categories` | ❌ No    | Main categories (array) | `[Tutorial, JavaScript]`    |
| `tags`       | ❌ No    | Specific tags (array)   | `[Jekyll, GitHub Pages]`    |
| `excerpt`    | ❌ No    | Custom excerpt          | `"Learn JavaScript basics"` |

### Step 3: Write Your Content

After the front matter, write your post using Markdown:

````markdown
---
layout: post
title: 'My New Blog Post'
date: 2026-02-15 00:00:00 +0800
categories: [Tutorial]
tags: [Jekyll, Blogging]
excerpt: 'Learn how to write blog posts with Jekyll'
---

# Main Heading

This is the first paragraph of your post. It will automatically become the excerpt if you don't specify one.

## Subheading

You can use all standard Markdown syntax:

- **Bold text**
- _Italic text_
- `Code snippets`
- [Links](https://example.com)

### Code Blocks

```javascript
function hello() {
  console.log('Hello, World!');
}
```
````

### Lists

1. First item
2. Second item
3. Third item

### Blockquotes

> This is a blockquote. Great for highlighting important information.

### Images

![Alt text](/images/your-image.jpg)

````

### Step 4: Commit and Push

```bash
git add _posts/2026-02-15-my-new-post.md
git commit -m "Add new blog post: My New Blog Post"
git push origin main
````

GitHub Pages will automatically rebuild your site within 1-2 minutes!

## 📝 Best Practices for Blog Posts

### 1. Use Descriptive Titles

Good titles are clear, specific, and include keywords:

- ✅ **Good:** "How to Set Up Jekyll on GitHub Pages"
- ✅ **Good:** "10 JavaScript Best Practices for Beginners"
- ❌ **Bad:** "My Post" or "Tutorial"

### 2. Write Compelling Excerpts

Excerpts appear in post previews and search results:

- Keep under 160 characters
- Make them engaging to encourage clicks
- Include the main value proposition
- Use active language

**Examples:**

- ✅ "Learn how to set up a Jekyll blog on GitHub Pages in 10 minutes with this complete step-by-step guide."
- ❌ "This is a post about Jekyll setup."

### 3. Use Categories and Tags Effectively

**Categories** are broad topics (1-3 per post):

- Tutorial
- JavaScript
- DevOps
- Web Development

**Tags** are specific keywords (3-5 per post):

- Jekyll
- GitHub Pages
- Markdown
- Static Sites
- Blogging

### 4. Structure Your Content

Organize your posts for readability:

```markdown
# Introduction

- Hook the reader
- State the problem
- Preview the solution

## Main Content

- Use headings (##, ###) to break up text
- Include code examples with syntax highlighting
- Add images and diagrams
- Use bullet points for lists

## Conclusion

- Summarize key points
- Provide next steps
- Include call-to-action
```

### 5. Optimize for SEO

- Use descriptive titles with keywords
- Include relevant keywords naturally in content
- Add meta descriptions via front matter
- Use proper heading hierarchy (H1, H2, H3)
- Optimize images with alt text

### 6. Write for Your Audience

- Know your target audience
- Use clear, simple language
- Explain technical concepts
- Provide practical examples
- Include code snippets that work

### 7. Use Code Blocks Properly

Always specify the language for syntax highlighting:

```javascript
// Good - specifies language
function greet(name) {
  return `Hello, ${name}!`;
}
```

```
// Bad - no language specified
function greet(name) {
  return `Hello, ${name}!`;
}
```

### 8. Add Visual Elements

- Screenshots for tutorials
- Diagrams for complex concepts
- Code snippets with syntax highlighting
- Infographics for data
- GIFs for demonstrations

### 9. Test Before Publishing

- Preview locally (optional)
- Check that code blocks render correctly
- Verify all links work
- Test on mobile devices
- Check for typos and errors

### 10. Engage with Readers

- End posts with questions
- Encourage comments
- Provide related posts
- Include social sharing buttons
- Respond to comments promptly

## 🎨 Customizing Your Blog

### Changing Site Settings

Edit `_config.yml` to personalize your blog:

```yaml
# Site settings
title: My Awesome Blog
description: Sharing knowledge about tech and development
author: Your Name
email: your-email@example.com

# Social settings
twitter_username: your_twitter
github_username: your_username
```

### Changing Colors and Styles

Edit the CSS in `_layouts/default.html`:

```css
:root {
  --primary-color: #2563eb; /* Main accent color */
  --secondary-color: #1e40af; /* Darker accent */
  --bg-color: #f8fafc; /* Background color */
  --text-color: #1e293b; /* Text color */
}
```

### Adding Custom Pages

Create new `.md` or `.html` files in the root directory:

```markdown
---
layout: default
title: Contact
---

<div style="background: white; padding: 40px; border-radius: 12px;">
  <h1>Contact Me</h1>
  <p>Feel free to reach out!</p>
</div>
```

## 🧪 Testing Locally (Optional)

If you want to preview changes before pushing:

### Install Ruby

**Windows:**

1. Download from [rubyinstaller.org](https://rubyinstaller.org/)
2. Run installer and follow prompts

**Mac:**

```bash
brew install ruby
```

**Linux (Ubuntu/Debian):**

```bash
sudo apt-get install ruby-full
```

### Install Jekyll

```bash
gem install bundler jekyll
bundle install
```

### Start Local Server

```bash
bundle exec jekyll serve
```

Visit `http://localhost:4000` to see your site!

## 📤 Publishing Workflow

### Daily Blogging Workflow

1. **Write your post** in `_posts/YYYY-MM-DD-title.md`
2. **Test locally** (optional): `bundle exec jekyll serve`
3. **Commit changes:** `git add . && git commit -m "Add new post"`
4. **Push to GitHub:** `git push origin main`
5. **Wait 1-2 minutes** for deployment
6. **Visit your site** to see the live post!

### Version Control Best Practices

```bash
# Commit frequently
git add _posts/new-post.md
git commit -m "Add post about Jekyll setup"

# Use descriptive commit messages
git commit -m "Fix typo in about page"
git commit -m "Update homepage layout"
git commit -m "Add new category: JavaScript"

# Pull before pushing
git pull origin main
git push origin main
```

## 🔍 Common Issues and Solutions

### Issue: Post Not Appearing

**Possible causes:**

1. Filename doesn't match `YYYY-MM-DD-title.md` format
2. Date is in the future
3. Missing front matter
4. File not in `_posts/` directory

**Solution:** Check filename format and front matter syntax.

### Issue: Images Not Loading

**Solution:**

1. Create an `images/` folder at the root level
2. Reference images as `/images/filename.jpg` (with leading slash)
3. Ensure image filenames are lowercase with hyphens

### Issue: Build Failing on GitHub

**Solution:**

1. Go to repository **Settings** → **Pages**
2. Check the **Build and deployment** section
3. Review error logs
4. Common issues: invalid YAML, missing quotes in front matter

### Issue: Changes Not Reflecting

**Solution:**

1. Clear your browser cache
2. Wait 1-2 minutes for GitHub Pages to rebuild
3. Check that you pushed to the correct branch
4. Verify the file was committed

### Issue: Formatting Issues

**Solution:**

- Ensure proper indentation in YAML front matter
- Use quotes around strings with special characters
- Check for trailing spaces in code blocks
- Validate Markdown syntax

## 📚 Additional Resources

### Official Documentation

- [Jekyll Official Documentation](https://jekyllrb.com/docs/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Markdown Guide](https://www.markdownguide.org/)
- [Kramdown Syntax](https://kramdown.gettalong.org/syntax.html)

### Useful Tools

- [Jekyll Themes](https://jekyllthemes.io/) - Pre-built themes
- [Markdown Editor](https://stackedit.io/) - Online Markdown editor
- [Canva](https://www.canva.com/) - Create blog images
- [Grammarly](https://www.grammarly.com/) - Check your writing

### Communities

- [Jekyll Talk Forum](https://talk.jekyllrb.com/)
- [GitHub Community Forum](https://github.community/)
- [Stack Overflow - Jekyll Tag](https://stackoverflow.com/questions/tagged/jekyll)

## 🎓 Quick Reference

### Markdown Syntax

| Element    | Syntax                    | Example                        |
| ---------- | ------------------------- | ------------------------------ |
| Heading    | `# H1`, `## H2`, `### H3` | `## My Section`                |
| Bold       | `**text**`                | `**important**`                |
| Italic     | `*text*`                  | `*emphasis*`                   |
| Code       | `` `code` ``              | `` `variable` ``               |
| Code block | ` `language ```           | `javascript\n...`              |
| Link       | `[text](url)`             | `[Google](https://google.com)` |
| Image      | `![alt](url)`             | `![Logo](/logo.png)`           |
| List       | `- item` or `1. item`     | `- First item`                 |
| Blockquote | `> quote`                 | `> Important note`             |

### Front Matter Template

```yaml
---
layout: post
title: 'Your Post Title'
date: YYYY-MM-DD HH:MM:SS +0800
categories: [Category1, Category2]
tags: [tag1, tag2, tag3]
excerpt: 'Brief description'
---
```

### File Naming Convention

```
YYYY-MM-DD-post-title-with-hyphens.md
```

**Examples:**

- `2026-02-15-how-to-setup-jekyll.md`
- `2026-03-01-javascript-best-practices.md`
- `2026-04-10-my-experience-with-react.md`

## 💡 Pro Tips

1. **Write in Markdown first** - Use a Markdown editor like VS Code with extensions
2. **Keep posts organized** - Use descriptive filenames and consistent categories
3. **Use code blocks** - Always specify language for syntax highlighting
4. **Add images** - Create an `images/` folder and organize by post
5. **Review before publishing** - Test locally or preview in GitHub
6. **Backup your content** - Keep a copy of important posts
7. **Engage with readers** - End posts with questions or calls-to-action
8. **Post consistently** - Maintain a regular publishing schedule
9. **Promote your posts** - Share on social media and relevant communities
10. **Learn from analytics** - Use GitHub Pages stats or add analytics

## 🎉 Conclusion

Congratulations! You now have a fully functional Jekyll blog on GitHub Pages. This setup gives you:

- ✅ Free hosting
- ✅ Automatic HTTPS
- ✅ Version control
- ✅ Easy content management
- ✅ Customizable design
- ✅ Fast performance

Start writing your first post and share your knowledge with the world!

## 🆘 Need Help?

If you encounter issues:

1. Check the [Jekyll documentation](https://jekyllrb.com/docs/)
2. Review [GitHub Pages troubleshooting](https://docs.github.com/en/pages/troubleshooting-github-pages)
3. Search for your error message online
4. Ask in the [Jekyll community forums](https://talk.jekyllrb.com/)

---

Happy blogging! 🚀

**Found this guide helpful?** Share it with others who want to start their own blog!
