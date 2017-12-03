---
layout: post
title: 지킬에 카테고리와 태그 적용하기
categories: jekyll
tags: jekyll
---
포스팅한 글들을 분류하려면 카테고리와 태그를 적용해야겠다고 생각했다. 다른 지킬 블로그 중 내가 원하는 카테고리와 태그를 적용해 놓은 곳을 찾아서 동일하게 적용하면 되겠다고 생각했다. 
구글링을 통해 지킬 블로그를 몇군데 돌아다니며 참고할만한 적당한 블로그를 찾을 수 있었다.  

- 참고한 지킬 블로그 : [https://gaohaoyang.github.io](https://gaohaoyang.github.io)
- 깃허브 저장소 : [https://github.com/Gaohaoyang/gaohaoyang.github.io](https://github.com/Gaohaoyang/gaohaoyang.github.io)

## 기능 분석
참고 블로그 카테고리와 태그 기능을 분석하고 내 블로그에는 어떻게 적용할지 생각해봤다. 

### 사이드 바의 카테고리 & 태그 목록
참고 블로그처럼 사이드 바에 카테고리와 태그 목록을 배치할 것이다.
카테고리의 경우 해당 카테고리에 몇개의 포스트가 포함되어 있는지 카테고리 항목 오른쪽에 뱃지 형태로 표시한다.
태그의 경우 포스트가 많을수록 태그명의 글자 크기가 크게 표시된다. 
![사이드바 카테고리 & 태그 목록]({{ site.url }}/assets/jekyll-category-001.png)

### 카테고리 & 태그 페이지 
사이드 바에서 카테고리나 태그 항목을 클릭하면 목록 페이지로 이동한다. 이 페이지는 모든 전체 목록을 보여주고 있다. 

나는 전체 목록이 아닌 선택한 항목 대한 목록만 보여주는 방식으로 커스터마이징 할 것이다. 
![카테고리 목록]({{ site.url }}/assets/jekyll-category-002.png)

## 내 블로그에 적용

### 사이드바에 카테고리 & 태그 목록 추가

#### 기존 구조
지금 내 블로그는 lanyon이라는 테마로 되어있다.
상단 메뉴 버튼을 나오면 블로그 어디서든 사이드바가 나오는 구조다.
사이드 메뉴 하단에 카테고리 메뉴를 추가해보겠다.
![기존 사이드바]({{ site.url }}/assets/jekyll-category-003.png)

```

<nav class="content-ul" cate>

{% raw %} {% for category in site.categories %} {% endraw %}
{% raw %}    <a href="{{ root_url }}/{{ site.category_dir }}#{{ category | first }}"{% endraw %}
       class="sidebar-nav-item"
       {% raw %}cate="{{ category | first }}">{% endraw %}
                          <span class="name">
                              {% raw %}{{ category | first }}{% endraw %}
                          </span>
      {% raw %}<span class="badge">{{ category | last | size }}</span>{% endraw %}
    </a>
  {% raw %}{% endfor %}{% endraw %}
</nav>
 
```
