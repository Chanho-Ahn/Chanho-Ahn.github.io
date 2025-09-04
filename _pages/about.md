---
permalink: /
title: "Chanho Ahn"
author_profile: true
redirect_from: 
  - /about/
  - /about.html

publications:
  - title: "Test-Time Ensemble via Linear Mode Connectivity: A Path to Better Adaptation"
    authors: "Byungjai Kim, Chanho Ahn, Wissam J. Baddar, Kikyung Kim, Huijin Lee, Saehyun Ahn, Seungju Han, Sungjoo Suh, and Eunho Yang"
    venue: "ICLR 2025"
    thumb: "/images/pub/tte.png"
    pdf: "/files/tte.pdf"
  - title: "Sample-wise Label Confidence Incorporation for Learning with Noisy Labels"
    authors: "Chanho Ahn, Kikyung Kim, Ji-won Baek, Jongin Lim, and Seungju Han"
    venue: "ICCV 2023"
    thumb: "/images/pub/lnl.png"
    pdf: "/files/lnl.pdf"
  - title: "Growing a Brain with Sparsity-inducing Generation for Continual Learning"
    authors: "Hyundong Jin, Gyeong-hyeon Kim, Chanho Ahn, and Eunwoo Kim"
    venue: "ICCV 2023"
    thumb: "/images/pub/growing.png"
    pdf: "/files/growing.pdf"
  - title: "Biasadv: Bias-adversarial augmentation for model debiasing"
    authors: "Jongin Lim, Youngdong Kim, Byungjai Kim, Chanho Ahn, Jinwoo Shin, Eunho Yang, and Seungju Han"
    venue: "CVPR 2023"
    thumb: "/images/pub/biasadv.png"
    pdf: "/files/biasadv.pdf"
  - title: "Incremental Learning with Adaptive Model Search and a Nominal Loss Model"
    authors: "Chanho Ahn, Eunwoo Kim, and Songhwai Oh"
    venue: "IEEE Access, vol. 10, pp. 16052-16062"
    thumb: "/images/pub/lnl.png"
    pdf: "/files/lnl.pdf"
  - title: "Auto-VirtualNet: Cost-Adaptive Dynamic Architecture Search for Multi-task Learning"
    authors: "Eunwoo Kim, Chanho Ahn, and Songhwai Oh"
    venue: "Neurocomputing, vol. 442, pp. 116-124"
    thumb: "/images/pub/avn.png"
    pdf: "/files/avn.pdf"
  - title: "Graph-Matching-Based Correspondence Search for Nonrigid Point Cloud Registration"
    authors: "Seunggyu Chang, Chanho Ahn, Minsik Lee, and Songhwai Oh"
    venue: "Computer Vision and Image Understanding, vol. 192"
    thumb: "/images/pub/matching.png"
    pdf: "/files/matching.pdf"
  - title: "Deep Elastic Networks with Model Selection for Multi-Task Learning"
    authors: "Chanho Ahn, Eunwoo Kim, and Songhwai Oh"
    venue: "ICCV 2019"
    thumb: "/images/pub/den.png"
    pdf: "/files/den.pdf"
  - title: "Deep Virtual Networks for Memory Efficiency Inference of Multiple Tasks"
    authors: "Eunwoo Kim, Chanho Ahn, Philip H.S. Torr, and Songhwai Oh"
    venue: "CVPR 2019"
    thumb: "/images/pub/dvn.png"
    pdf: "/files/dvn.pdf"
  - title: "NestedNet: Learning Nested Sparse Structures in Deep Neural Networks"
    authors: "Eunwoo Kim, Chanho Ahn, and Songhwai Oh"
    venue: "CVPR 2018 (Spotlight)"
    thumb: "/images/pub/nestednet.png"
    pdf: "/files/nestednet.pdf"
---
<style>
.page__title{ display:none; }
.pub-card{
  display:flex; gap:1.25rem; align-items:flex-start;
  padding:1.1rem 1.25rem; background:#f6fbff;
  border:1px solid #e7f1ff; border-radius:18px;
  box-shadow:0 10px 24px rgba(30,60,90,.08); margin:1rem 0; font-size: 0.95rem;
}
.pub-card__img{ flex:0 0 260px; }
.pub-card__img img{ width:100%; height:auto; border-radius:14px; }
.pub-title{ margin:.1rem 0 .4rem; font-size:1.25rem; line-height:1.25; }
.pub-authors{ opacity:.9; margin-bottom:.35rem; }
.pub-venue{ color:#2761c0; margin-bottom:.5rem; }
.pub-links a{ display:inline-block; margin-right:.5rem; }
@media (max-width: 900px){
  .pub-card{ flex-direction:column; }
  .pub-card__img{ flex:0 0 auto; }
}
</style>

I am a Staff Researcher at the Samsung Advanced Institute of Technology (SAIT). I received a Ph.D. in Electrical and Computer Engineering from Seoul National University in 2022 and B.S. in Electrical and Computer Engineering from Seoul National University in 2016. My current research focuses on AI agent, vision foundation models, and vision–language models. My goal is to create real-world impact through practical algorithm development, with emphasis on efficient adaptation, robustness, and fine-grained visual understanding for industrial applications.

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
