---
layout: default
---
<div class="home">

  <h1 class="page-heading">Tagged: {{ page.slug }}</h1>

  {% if page.slug %}
    <ul class="post-list">
      {% for post in site.posts %}
        {% if post.tags contains page.slug %}
          <li>
            <span class="post-meta">
              {{ post.date | date: "%b %-d, %Y" }} • Tags: 
              {% for tag in post.tags %}
                <a href="{{ 'tag/' | relative_url }}{{ tag }}">{{ tag }}</a>
              {% endfor %}
            </span>
            <h2>
              <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
            </h2>
          </li>
        {% endif %}
      {% endfor %}
    </ul>
  {% else %}
    <p class="article-empty">
      There are no posts in {{ page.slug }}.
    </p>
  {% endif %}
</div>
