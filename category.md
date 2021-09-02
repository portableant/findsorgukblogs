---
layout: default
title: Categories
---

<div class="tags-expo">
    <div class="tags-expo-list">
        <h2>Categories in use</h2>
        {% for tag in site.categories %}
        <a href="#{{ tag[0] | slugify }}" class="post-tag">{{ tag[0] }}</a>
        {% endfor %}
    </div>
    <hr/>
    <div class="tags-expo-section">
        {% for tag in site.categories %}
        <h2 id="{{ tag[0] | slugify }}">{{ tag[0] }}</h2>
        <ul>
            {% for post in tag[1] %}

                <li>
                    <a href="{{ site.baseurl }}{{ post.url }}">
                    {{ post.title }}
                    <small class="post-date">{{ post.date | date_to_string }}</small>
                    </a>
                </li>

            {% endfor %}
        </ul>
        {% endfor %}
    </div>
</div>