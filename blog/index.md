---
layout: archive
permalink: /blog/
title: "Blog"
author_profile: false
---

<style>
.archive {
  max-width: 1200px;
  margin: 0 auto;
  padding: 40px 20px;
}

.archive h1 {
  font-size: 2rem;
  margin-bottom: 3rem;
  text-align: center;
  color: #4CAF50;
  font-weight: 800;
}

.post-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 2rem;
  list-style: none;
  padding: 0;
}

.post-item {
  background: linear-gradient(135deg, rgba(45,50,60,1) 0%, rgba(35,40,50,1) 100%);
  border: 1px solid rgba(76, 175, 80, 0.3);
  border-radius: 8px;
  padding: 1.5rem;
  transition: all 0.3s ease;
  display: flex;
  flex-direction: column;
}

.post-item:hover {
  border-color: #4CAF50;
  box-shadow: 0 8px 20px rgba(76, 175, 80, 0.2);
  transform: translateY(-4px);
}

.post-date {
  font-size: 0.85rem;
  color: #4CAF50;
  font-weight: 600;
  margin-bottom: 0.5rem;
}

.post-title {
  font-size: 1.3rem;
  font-weight: 700;
  color: white;
  margin-bottom: 1rem;
  text-decoration: none;
  transition: color 0.3s ease;
}

.post-item a:hover .post-title {
  color: #66BB6A;
}

.post-excerpt {
  color: rgba(255,255,255,0.7);
  flex-grow: 1;
  margin-bottom: 1rem;
  line-height: 1.6;
}

.post-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-top: 1rem;
}

.tag {
  background: rgba(76, 175, 80, 0.2);
  color: #4CAF50;
  padding: 0.3rem 0.6rem;
  border-radius: 4px;
  font-size: 0.8rem;
  text-decoration: none;
  transition: all 0.2s ease;
}

.tag:hover {
  background: rgba(76, 175, 80, 0.4);
  color: #66BB6A;
}

.read-more {
  color: #4CAF50;
  text-decoration: none;
  font-weight: 600;
  margin-top: 1rem;
}

.read-more:hover {
  color: #66BB6A;
}
</style>

<div class="archive">
  <h1>🤖 Latest Posts</h1>
  
  <ul class="post-list">
    {% for post in site.blog reversed %}
      <li class="post-item">
        <a href="{{ post.url }}" style="text-decoration: none; color: inherit;">
          <div class="post-date">📅 {{ post.date | date: "%b %d, %Y" }}</div>
          <h2 class="post-title">{{ post.title }}</h2>
          <p class="post-excerpt">{{ post.excerpt }}</p>
          <span class="read-more">Read More →</span>
        </a>
        {% if post.tags %}
          <div class="post-tags">
            {% for tag in post.tags %}
              <a href="/blog/tag/{{ tag | slugify }}/" class="tag">#{{ tag }}</a>
            {% endfor %}
          </div>
        {% endif %}
      </li>
    {% endfor %}
  </ul>

  {% if site.blog.size == 0 %}
    <p style="text-align: center; color: rgba(255,255,255,0.6);">No posts yet. Check back soon!</p>
  {% endif %}
</div>
