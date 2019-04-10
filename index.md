---
layout: page
title: Welcome!
---


I am a Computational Biologist at Fred Hutch. 

My expertise include: S4 classes for assay data, Machine-learning (tuning, benchmarking, resampling, ensemble), meta-analysis and inferential models.  

<img src="https://drorberel.github.io/img/paradigmIII.jpg">



## Recent projects: 

### Meta analysis of multi-assay data: proof of concept
poster: Prototype meta-analysis demonstration for ImmuneSpaceR, using designated S4 objects
[https://www.bioconductor.org/help/course-materials/2017/BioC2017/DDay/LightningTalk/SessionII/ImmuneSpaceR.pdf](https://www.bioconductor.org/help/course-materials/2017/BioC2017/DDay/LightningTalk/SessionII/ImmuneSpaceR.pdf)


### Bioc2mlr
R package to bridge between Bioconductor’s S4 complex genomic data container, to mlr, a meta machine learning aggregator package. 

[https://github.com/drorberel/Bioc2mlr](https://github.com/drorberel/Bioc2mlr)


## REcent Conference Presentations
- [ASA SDSS Symposium on Data Science and Statistics, 2019, Bellevue, WA](https://ww2.amstat.org/meetings/sdss/2019/onlineprogram/AbstractDetails.cfm?AbstractID=306196).
- [CascadiaR 2018](https://cascadiarconf.com/years/2018/agenda/#dror-berel)  
- [ASA JSM, Vancouver,BC 2018](https://ww2.amstat.org/meetings/jsm/2018/onlineprogram/AbstractDetails.cfm?abstractid=328658)  
- [CascadiaR 2017](https://cascadiarconf.com/years/2017/agenda/)
- [BioC 2017](https://bioconductor.org/help/course-materials/2017/BioC2017/)




<div class="posts-list">
  {% for post in paginator.posts %}
  <article class="post-preview">
    <a href="{{ post.url | prepend: site.baseurl }}">
	  <h2 class="post-title">{{ post.title }}</h2>

	  {% if post.subtitle %}
	  <h3 class="post-subtitle">
	    {{ post.subtitle }}
	  </h3>
	  {% endif %}
    </a>

    <p class="post-meta">
      Posted on {{ post.date | date: "%B %-d, %Y" }}
    </p>

    <div class="post-entry-container">
      {% if post.image %}
      <div class="post-image">
        <a href="{{ post.url | prepend: site.baseurl }}">
          <img src="{{ post.image }}">
        </a>
      </div>
      {% endif %}
      <div class="post-entry">
        {{ post.excerpt | strip_html | xml_escape | truncatewords: site.excerpt_length }}
        {% assign excerpt_word_count = post.excerpt | number_of_words %}
        {% if post.content != post.excerpt or excerpt_word_count > site.excerpt_length %}
          <a href="{{ post.url | prepend: site.baseurl }}" class="post-read-more">[Read&nbsp;More]</a>
        {% endif %}
      </div>
    </div>

    {% if post.tags.size > 0 %}
    <div class="blog-tags">
      Tags:
      {% if site.link-tags %}
      {% for tag in post.tags %}
      <a href="{{ site.baseurl }}/tags#{{- tag -}}">{{- tag -}}</a>
      {% endfor %}
      {% else %}
        {{ post.tags | join: ", " }}
      {% endif %}
    </div>
    {% endif %}

   </article>
  {% endfor %}
</div>

{% if paginator.total_pages > 1 %}
<ul class="pager main-pager">
  {% if paginator.previous_page %}
  <li class="previous">
    <a href="{{ paginator.previous_page_path | prepend: site.baseurl | replace: '//', '/' }}">&larr; Newer Posts</a>
  </li>
  {% endif %}
  {% if paginator.next_page %}
  <li class="next">
    <a href="{{ paginator.next_page_path | prepend: site.baseurl | replace: '//', '/' }}">Older Posts &rarr;</a>
  </li>
  {% endif %}
</ul>
{% endif %}