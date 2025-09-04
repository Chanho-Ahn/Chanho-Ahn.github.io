---
permalink: /
title: "Chanho Ahn"
author_profile: true
redirect_from: 
  - /about/
  - /about.html

publications:
  - title: "Semantic Environment Atlas for Object-Goal Navigation"
    authors: "Nuri Kim, Jeongho Park, Mineui Hong, and Songhwai Oh"
    venue: "Knowledge-Based Systems, Vol. 304, 2024"
    thumb: "/images/pub/den.png"
    pdf: "/files/den.pdf"
  - title: "다음 논문 제목"
    authors: "Chanho Ahn, ..."
    venue: "CVPR 2025 (to appear)"
    thumb: "/images/pub/avn.png"
    pdf: "/files/avn.pdf"
---
<style>
.pub-card{
  display:flex; gap:1.25rem; align-items:flex-start;
  padding:1.1rem 1.25rem; background:#f6fbff;
  border:1px solid #e7f1ff; border-radius:18px;
  box-shadow:0 10px 24px rgba(30,60,90,.08); margin:1rem 0;
}
.pub-card__img{ flex:0 0 260px; }
.pub-card__img img{ width:100%; height:auto; border-radius:14px; }
.pub-title{ margin:.1rem 0 .4rem; font-size:1.6rem; line-height:1.25; }
.pub-authors{ opacity:.9; margin-bottom:.35rem; }
.pub-venue{ color:#2761c0; margin-bottom:.5rem; }
.pub-links a{ display:inline-block; margin-right:.5rem; }
@media (max-width: 900px){
  .pub-card{ flex-direction:column; }
  .pub-card__img{ flex:0 0 auto; }
}
</style>

<h2 id="publications">Publications</h2>
{% for p in page.publications %}
  <div class="pub-card">
    {% if p.thumb %}
    <div class="pub-card__img">
      <img src="{{ p.thumb }}" alt="{{ p.title | escape }}">
    </div>
    {% endif %}
    <div class="pub-card__body">
      <h3 class="pub-title">{{ p.title }}</h3>
      {% if p.authors %}<div class="pub-authors">{{ p.authors | replace: 'Chanho Ahn', '<strong>Chanho Ahn</strong>' }}</div>{% endif %}
      {% if p.venue %}<div class="pub-venue">{{ p.venue }}</div>{% endif %}
      <p class="pub-links">
        {% if p.pdf %}<a class="btn btn--primary" href="{{ p.pdf }}">PDF</a>{% endif %}
        {% if p.arxiv %}<a class="btn" href="{{ p.arxiv }}">arXiv</a>{% endif %}
        {% if p.code %}<a class="btn" href="{{ p.code }}">Code</a>{% endif %}
      </p>
    </div>
  </div>
{% endfor %}
