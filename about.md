---
layout: default
title: About
---

<div class="row">
  <div class="col-lg-8">
    <div class="post-content">
      <h1>About This Blog</h1>
      
      <p>Welcome to my personal blog! This is a space where I share my thoughts, experiences, and insights about technology, software development, and everything in between.</p>
      
      <h2>About Me</h2>
      <p>I'm a passionate developer who loves exploring new technologies and sharing knowledge with the community. This blog serves as my platform to document what I learn and help others along the way.</p>
      
      <h2>What You'll Find Here</h2>
      <ul>
        <li>Tutorials and guides on various programming topics</li>
        <li>Deep dives into specific technologies and frameworks</li>
        <li>Personal projects and their development journey</li>
        <li>Best practices and tips for developers</li>
        <li>Thoughts on industry trends and emerging technologies</li>
      </ul>
      
      <h2>Get In Touch</h2>
      <p>I'd love to hear from you! Feel free to reach out through:</p>
      <ul>
        <li><a href="https://github.com/{{ site.github_username }}" target="_blank">GitHub</a></li>
        {% if site.twitter_username %}
        <li><a href="https://twitter.com/{{ site.twitter_username }}" target="_blank">Twitter</a></li>
        {% endif %}
        {% if site.email %}
        <li><a href="mailto:{{ site.email }}">Email</a></li>
        {% endif %}
      </ul>
      
      <p>Thanks for stopping by, and I hope you find something useful here!</p>
    </div>
  </div>
  
  <div class="col-lg-4">
    <aside>
      <!-- Quick Links -->
      <div class="sidebar">
        <h4 class="sidebar-title">Quick Links</h4>
        <ul class="sidebar-list">
          <li>
            <a href="{{ site.baseurl }}/">
              <i class="fas fa-home"></i> Home
            </a>
          </li>
          <li>
            <a href="{{ site.baseurl }}/about">
              <i class="fas fa-user"></i> About
            </a>
          </li>
          <li>
            <a href="https://github.com/{{ site.github_username }}" target="_blank">
              <i class="fab fa-github"></i> GitHub
            </a>
          </li>
        </ul>
      </div>
      
      <!-- Recent Posts -->
      <div class="sidebar">
        <h4 class="sidebar-title">Recent Posts</h4>
        <ul class="sidebar-list">
          {% for post in site.posts limit: 5 %}
          <li>
            <a href="{{ site.baseurl }}{{ post.url }}">
              {{ post.title }}
            </a>
            <small class="text-muted d-block">{{ post.date | date: "%b %d, %Y" }}</small>
          </li>
          {% endfor %}
        </ul>
      </div>
    </aside>
  </div>
</div>
