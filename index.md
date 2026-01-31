---
layout: default
title: Home
---

<div class="row">
  <div class="col-lg-8">
    <h2 class="mb-4">Latest Posts</h2>
    
    {% for post in site.posts %}
    <article class="post-card">
      <h3 class="post-title">
        <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
      </h3>
      <div class="post-meta">
        <i class="far fa-calendar-alt"></i> {{ post.date | date: "%B %d, %Y" }}
        {% if post.author %}
        <span class="mx-2">|</span>
        <i class="far fa-user"></i> {{ post.author }}
        {% endif %}
        {% if post.tags %}
        <span class="mx-2">|</span>
        <i class="fas fa-tags"></i>
        {% for tag in post.tags %}
          <span class="tag">{{ tag }}</span>
        {% endfor %}
        {% endif %}
      </div>
      <div class="post-excerpt">
        {% if post.excerpt %}
          {{ post.excerpt | strip_html | truncatewords: 50 }}
        {% else %}
          {{ post.content | strip_html | truncatewords: 50 }}
        {% endif %}
      </div>
      <div class="mt-3">
        <a href="{{ site.baseurl }}{{ post.url }}" class="read-more">
          Read More <i class="fas fa-arrow-right"></i>
        </a>
      </div>
    </article>
    {% endfor %}
    
    <!-- Pagination -->
    {% if paginator.total_pages > 1 %}
    <nav aria-label="Page navigation" class="mt-4">
      <ul class="pagination justify-content-center">
        {% if paginator.previous_page %}
        <li class="page-item">
          <a class="page-link" href="{{ site.baseurl }}{{ paginator.previous_page_path }}">
            <i class="fas fa-chevron-left"></i> Previous
          </a>
        </li>
        {% endif %}
        
        {% for page in (1..paginator.total_pages) %}
          {% if page == paginator.page %}
          <li class="page-item active">
            <span class="page-link">{{ page }}</span>
          </li>
          {% elsif page == 1 or page == paginator.total_pages or (page >= paginator.page | minus: 1 and page <= paginator.page | plus: 1) %}
          <li class="page-item">
            <a class="page-link" href="{{ site.baseurl }}{{ site.paginate_path | replace: ':num', page }}">{{ page }}</a>
          </li>
          {% elsif page == paginator.page | minus: 2 or page == paginator.page | plus: 2 %}
          <li class="page-item disabled">
            <span class="page-link">...</span>
          </li>
          {% endif %}
        {% endfor %}
        
        {% if paginator.next_page %}
        <li class="page-item">
          <a class="page-link" href="{{ site.baseurl }}{{ paginator.next_page_path }}">
            Next <i class="fas fa-chevron-right"></i>
          </a>
        </li>
        {% endif %}
      </ul>
    </nav>
    {% endif %}
  </div>
  
  <div class="col-lg-4">
    <aside>
      <!-- About -->
      <div class="sidebar">
        <h4 class="sidebar-title">About This Blog</h4>
        <p>{{ site.description }}</p>
        <a href="{{ site.baseurl }}/about" class="btn btn-primary btn-sm">
          Learn More <i class="fas fa-arrow-right"></i>
        </a>
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
      
      <!-- Tags -->
      {% if site.tags %}
      <div class="sidebar">
        <h4 class="sidebar-title">Tags</h4>
        <div>
          {% for tag in site.tags %}
            <a href="{{ site.baseurl }}/tags/{{ tag[0] }}/" class="tag">{{ tag[0] }}</a>
          {% endfor %}
        </div>
      </div>
      {% endif %}
    </aside>
  </div>
</div>
