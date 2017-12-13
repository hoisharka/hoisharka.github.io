---
layout: post
title: 지킬에 카테고리와 태그 적용하기 (2)
categories: jekyll
tags: jekyll
---
사이드바에서 카테고리와 태그 페이지로 이동하는 기능을 구현한다.
현재 카테고리 항목을 클릭하면 `[블로그 주소]/category/#[카테고리명]`로 이동한다.
마찬가지로 태그를 누르면 `[블로그 주소]/tag/#[태그명]` 으로 이동한다.

## 카테고리 페이지 추가

- 블로그 루트 디렉토리에 `page` 디렉토리를 추가한다.
- page 디렉토리 안에 `1.category.html`라는 파일을 추가한다.
- YAML 머리말을 추가한다. 이 부분을 추가함으로써 사이드바의 카테고리 항목을 클릭하면 `1.category.html`로 이동된다.
```yaml
---
layout: default
title: Categories
permalink: /category/
icon: th-list
type: page
---
```

- 아래 내용을 더 쓰면 일단 전체 카테고리 목록은 나온다

``` liquid
{% raw %}
<div class="page clearfix">
  <div class="left">
    <h1>{{page.title}}</h1>
    <hr>
    <ul>
      {% for category in site.categories %}
      <h2 id="{{category | first}}">{{category | first}}</h2>
      {% for posts in category %}
      {% for post in posts %}
      {% if post.url %}
      <li>
        <time>
          {{ post.date | date:"%F" }} {{ post.date | date: "%a" }}.
        </time>
        <a class="title" href="{{ post.url | prepend: site.url }}">{{ post.title }}</a>
      </li>
      {% endif %}
      {% endfor %}
      {% endfor %}
      {% endfor %}
    </ul>
  </div>
</div>
{% endraw %}
```
![전체 카테고리 목록]({{ site.url }}/assets/jekyll-category-006.png)

계획대로라면 단일 카테고리에 관련된 포스트 목록만 보여주는 것이 목표였다.
그러려면 url 끝에 앵커 부분 지정자(#[카테고리명])를 `liquid` 영역에서 다룰 수 있어야한다. 
해당 기능을 검색해봤는데, 안타깝게도 `liquid` 영역에는 그런 기능이 없는 듯하다.

대안책으로 vue.js를 사용하는 방법을 찾을 수 있었다.
[* 참고 블로그](https://cookieshake.github.io/posts/Jekyll-%EB%B8%94%EB%A1%9C%EA%B7%B8%EC%97%90-Category-%ED%8E%98%EC%9D%B4%EC%A7%80-%EB%A7%8C%EB%93%A4%EA%B8%B0) 

구형 브라우저에 대한 호환성이 없다고는 하는데, 구형 브라우저 지원까지 할 생각은 없어서 적용해보기로 했다.

루트 디렉토리에 `posts.json`을 만들고 아래 코드를 추가한다.
```liquid
{% raw %}
---
---
{% capture json %}
{
  "posts": [
    {% for post in site.posts %}
    {
      "title": "{{ post.title }}",
      "date": "{{ post.date | date: "%Y-%m-%d"}}",
      "categories": {{ post.categories | jsonify }},
      "url": "{{ post.url }}"
    }
    {% unless forloop.last %},{% endunless %}
    {% endfor -%}
  ]
}
{% endcapture %}
{{ json | strip_newlines | normalize_whitespace }}
{% endraw %}
```
_include/head.html에 해더에 아래 스크립트를 삽입한다.
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.2.6/vue.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/axios/0.16.0/axios.min.js"></script>
```

/page/1.category.html 을 수정한다.

```liquid
{% raw %}
---
layout: default
title: Categories
permalink: /category/
type: page
---
{% assign esc_title="{{ p.title }}" %}
{% assign esc_date="{{ p.date }}" %}
{% assign esc_ctgname="{{ hash }}" %}

<div id="post-list">
  <h1>{{page.title}}</h1>
  <hr>
  <h2 id="{{ category | first }}">{{ esc_ctgname }}</h2>
  <ul>
    <li v-for="p in posts" v-if="p.categories.indexOf(hash) != -1">
      <time>
        {{ esc_date }}
      </time>
      <a v-bind:href="p.url">{{ esc_title }}</a>
    </li>
  </ul>
</div>

<script>
  var hash = decodeURI(window.location.hash.substr(1));

  var postList = new Vue({
    el: '#post-list',
    data: {
      posts: []
    }
  });

  axios.get('/posts.json')
  .then(function (response) {
    postList.posts = response.data.posts;
  })
  .catch(function (error) {
    console.log(error);
  });
</script>

{% endraw %}

```

vue.js를 잘은 알지 못해서 정확히 어떻게 돌아가는지는 모르겠지만 분석해봤다.
- `axios.get`을 통해 `posts.json`의 카테고리 데이터를 `json`형식으로 가져와서 `vue`객체의 `data.posts`에 할당했다.
- li 태그의 `v-for="p in posts"`를 통해 루프를 돌며 `p`변수에 포스트 데이터를 할당하고 있는 듯하다.
- `v-if="p.categories.indexOf(hash) != -1"` 구문으로 `hash`값과 일치하는 카테고리의 포스트만 목록으로 보여주었다.
- 그 외에 `v-for`구문 밖에서 `p.title`과 같이 `p`변수에 접근하는 것과 같은 구문은 이해가 잘 되지 않는다. 동작 순서도 정확히는 잘 모르겠다.
- `vue`와 `liquid`를 좀 더 공부해야 확실하게 이해할 수 있을 것 같은데, 일단은 패스한다. 

완성된 카테고리 페이지이다.
![카테고리 페이지 완성]({{ site.url }}/assets/jekyll-category-007.png)

## 테그 페이지 추가
카테고리 페이지 작성과 거의 동일하다. 주요 소스만 적어보겠다.

일단 `posts.json`파일에는 카테고리 정보만 있다. 태그 정보도 카테고리 정보 밑에 추가한다.

```liquid
{% raw %}
...
"categories": {{ post.categories | jsonify }},
"tags": {{ post.tags | jsonify }},
...
{% endraw %}

```

/page/2.tag.html 파일을 생성하고 아래 코드를 추가한다.

```liquid
{% raw %}
---
layout: default
title: tags
permalink: /tag/
icon: th-list
type: page
---
{% assign esc_title="{{ p.title }}" %}
{% assign esc_date="{{ p.date }}" %}
{% assign esc_tagname="{{ hash }}" %}

<div id="post-list">
  <h1>{{page.title}}</h1>
  <hr>
  <h2 id="{{ category | first }}"> {{ esc_tagname }} </h2>
  <ul>
    <li v-for="p in posts" v-if="p.tags.indexOf(hash) != -1">
      <time>
        {{ esc_date }}
      </time>
      <a v-bind:href="p.url">{{ esc_title }}</a>
    </li>
  </ul>
</div>

<script>
  var hash = decodeURI(window.location.hash.substr(1));

  var postList = new Vue({
    el: '#post-list',
    data: {
      posts: []
    }
  });

  axios.get('/posts.json')
  .then(function (response) {
    postList.posts = response.data.posts;
  })
  .catch(function (error) {
    console.log(error);
  });
</script>

{% endraw %}
```

테그 페이지도 완성됐다.
![테그 페이지 완성]({{ site.url }}/assets/jekyll-category-008.png)

- 참고
  - [https://cookieshake.github.io/posts/Jekyll-블로그에-Category-페이지-만들기](https://cookieshake.github.io/posts/Jekyll-%EB%B8%94%EB%A1%9C%EA%B7%B8%EC%97%90-Category-%ED%8E%98%EC%9D%B4%EC%A7%80-%EB%A7%8C%EB%93%A4%EA%B8%B0)
