---
layout: post
title: 지킬에 카테고리와 태그 적용하기 (1)
categories: jekyll
tags: jekyll
---
포스팅한 글들을 분류하려면 카테고리와 태그를 적용해야겠다고 생각했다. 
다른 지킬 블로그 중 내가 원하는 카테고리와 태그를 적용해 놓은 곳을 찾아보았다.
구글링을 통해 지킬 블로그를 몇군데 돌아다니다가 참고할만한 적당한 블로그를 찾을 수 있었다.  

- 참고한 지킬 블로그 : [https://gaohaoyang.github.io](https://gaohaoyang.github.io)
- 깃허브 저장소 : [https://github.com/Gaohaoyang/gaohaoyang.github.io](https://github.com/Gaohaoyang/gaohaoyang.github.io)

## 기능 분석
참고 블로그 카테고리와 태그 기능을 분석하고 내 블로그에는 어떻게 적용할지 생각해봤다. 

### 사이드 바의 카테고리 & 태그 목록
참고 블로그처럼 사이드 바에 카테고리와 태그 목록을 배치할 것이다.
카테고리의 경우 해당 카테고리에 몇개의 포스트가 포함되어 있는지 
카테고리 항목 오른쪽에 뱃지 형태로 표시한다.
태그의 경우 포스트가 많을수록 태그명의 글자 크기가 크게 표시된다. 
![사이드바 카테고리 & 태그 목록]({{ site.url }}/assets/jekyll-category-001.png)

### 카테고리 & 태그 페이지 
사이드 바에서 카테고리나 태그 항목을 클릭하면 목록 페이지로 이동한다. 
이 페이지는 모든 전체 목록을 보여주고 있다. 

나는 전체 목록이 아닌 선택한 항목 대한 목록만 보여주는 방식으로 커스터마이징 할 것이다. 
![카테고리 목록]({{ site.url }}/assets/jekyll-category-002.png)

## 내 블로그에 적용

### _config.yml 수정
먼저 카테고리나 태그 항목을 클릭했을 때 이동할 경로를 지정해야 한다.
_config.yml에 아래 코드를 추가하면 된다.
```yaml
category_dir: category/
tag_dir: tag/
```

### 사이드바에 카테고리목록 추가

지금 내 블로그는 lanyon이라는 테마로 되어있다.
상단 메뉴 버튼을 나오면 블로그 어디서든 사이드바가 나오는 구조다.
사이드 메뉴 하단에 카테고리 메뉴를 추가해보겠다.
![기존 사이드바]({{ site.url }}/assets/jekyll-category-003.png)

아래 소스를 _include/sidebar.html의 사이드바 하단에 아래 코드를 추가했다.

```liquid
{% raw %}
<div>
  <div class="sidebar-nav-item">Category</div>
  <nav class="content-ul">
    {% for category in site.categories %}
    <a href="{{ root_url }}/{{ site.category_dir }}#{{ category | first }}"
       class="sidebar-nav-item pl-3rem">
                          <span class="name">
                              {{ category | first }}
                          </span>
      <span class="badge">{{ category | last | size }}</span>
    </a>
    {% endfor %}
  </nav>
</div>
{% endraw %}
```
코드를 분석해보자. 
- 카테고리 정보는 {% raw %} `{{site.categories}}` {% endraw %}에 들어있다.
- {% raw %} `{{site.categories}}` {% endraw %} 를 그냥 찍어보면 아래처럼 나온다.
```
{“ubuntu”=>[#<Jekyll::Document _posts/2017-11-20-ubuntu-base-setting.md collection=posts>], “jekyll”=>[#<Jekyll::Document _posts/2017-12-03-jekyll-category.md collection=posts>, #<Jekyll::Document _posts/2017-12-01-jekyll-start.md collection=posts>]}
```
- `"카테고리명"=>[포스트 항목1, 포스트 항목2 ...]` 형태를 하나의 카테고리 정보라고 볼 수 있다.
- for 루프를 돌며 `site.categories`의 항목들을 `category`라는 변수에 배치해서 작업한다.
- 카테고리 명은 {% raw %} `{{ category | first }}` {% endraw %} 으로 추출한다.
- 카테고리에 포함된 포스트 갯수는 {% raw %} `{{ category | last | size}}` {% endraw %} 으로 추출한다. 
- 카테고리명을 사용해 목록을 구성하고 각 항목의 링크도 지정한다.( `site.category_dir`은 _config.yml에 추가한 `/category`이다.)
- 포스트 갯수를 뱃지 UI로 나타낸다.

### 사이드바에 태그 목록 추가

추가한 카테고리 목록 영역 아래부분에 태그 목록 코드를 추가한다.

```liquid
{% raw %}
<div>
  <div class="sidebar-nav-item">Tags</div>
  <div class="tags-cloud">
    <div>
      {% assign first = site.tags.first %}
      {% assign max = first[1].size %}
      {% assign min = max %}
      {% for tag in site.tags offset:1 %}
      {% if tag[1].size > max %}
      {% assign max = tag[1].size %}
      {% elsif tag[1].size < min %}
      {% assign min = tag[1].size %}
      {% endif %}
      {% endfor %}

      {% if max == min %}
      {% assign diff = 1 %}
      {% else %}
      {% assign diff = max | minus: min %}
      {% endif %}

      {% for tag in site.tags %}
      {% assign temp = tag[1].size | minus: min | times: 36 | divided_by: diff %}
      {% assign base = temp | divided_by: 4 %}
      {% assign remain = temp | modulo: 4 %}
      {% if remain == 0 %}
      {% assign size = base | plus: 9 %}
      {% elsif remain == 1 or remain == 2 %}
      {% assign size = base | plus: 9 | append: '.5' %}
      {% else %}
      {% assign size = base | plus: 10 %}
      {% endif %}
      {% if remain == 0 or remain == 1 %}
      {% assign color = 9 | minus: base %}
      {% else %}
      {% assign color = 8 | minus: base %}
      {% endif %}
      <a href="{{ root_url }}/{{ site.tag_dir }}#{{ tag[0] }}" style="font-size: {{ size }}pt; color: #{{ 9 | minus: color }}{{ 9 | minus: color }}{{ 9 | minus: color }};">{{ tag[0] }}</a>
      {% endfor %}
    </div>
  </div>
</div>
{% endraw %}
```

카테고리보다 코드가 복잡하다. 카테고리는 단순하게 for루프를 돌며 목록을 나열하는데, 태그는 해당 태그에 속한 포스트의 갯수에 따라 태그명의 글자 크기와 글자색을 다르게 나타내고 있다.

코드의 첫부분을 분석해보자.
```liquid
{% raw %}
{% assign first = site.tags.first %}
{% assign max = first[1].size %}
{% assign min = max %}
{% for tag in site.tags offset:1 %}
{% if tag[1].size > max %}
{% assign max = tag[1].size %}
{% elsif tag[1].size < min %}
{% assign min = tag[1].size %}
{% endif %}
{% endfor %}
{% endraw %}
```
- 작업 당시 `site.tags`를 찍어보면 아래처럼 구성되어있다.
카테고리와 동일하게 지정해놔서 내용도 동일하다.
```
{“ubuntu”=>[#<Jekyll::Document _posts/2017-11-20-ubuntu-base-setting.md collection=posts>], “jekyll”=>[#<Jekyll::Document _posts/2017-12-03-jekyll-category.md collection=posts>, #<Jekyll::Document _posts/2017-12-01-jekyll-start.md collection=posts>]}
```
- 첫번째 태그 항목을 `first' 변수에 할당한다.
- `max`는 `first[1].size` 값을 할당한다. `first[1].size`는 첫번째 태그 항목에 속한 포스트의 갯수이다.
- `min`의 초기값은 `max`와 동일하다.
- for루프를 돌며 `max`와 `min`을 갱신한다.
  - 태그 항목의 포스트 갯수가 `max`보다 많으면 해당 태그 항목의 포스트 갯수를 `max`로 갱신한다.
  - 태그 항목의 포스트 갯수가 `min`보다 작으면 해당 태그 항목의 포스트 갯수를 `min`로 갱신한다.
- 간단히 말하면 `max`에는 가장 많은 포스트 갯수가 담기겠고 `min`에는 가장 적은 포스트 갯수가 담기겠다.
  
다음 구문을 살펴보자
```
{% raw %}
{% if max == min %}
{% assign diff = 1 %}
{% else %}
{% assign diff = max | minus: min %}
{% endif %}
{% endraw %}
```
- `diff`를 할당하는 것이 전부다
- `max`와 `min`이 같으면 `diff`가 1, 아니면 `diff`는 `max` - `min`이다.

`size` 값 할당 부분을 살펴보자.
```
{% raw %}
{% for tag in site.tags %}
{% assign temp = tag[1].size | minus: min | times: 36 | divided_by: diff %}
{% assign base = temp | divided_by: 4 %}
{% assign remain = temp | modulo: 4 %}
{% if remain == 0 %}
{% assign size = base | plus: 9 %}
{% elsif remain == 1 or remain == 2 %}
{% assign size = base | plus: 9 | append: '.5' %}
{% else %}
{% assign size = base | plus: 10 %}
{% endif %}

{% if remain == 0 or remain == 1 %}
{% assign color = 9 | minus: base %}
{% else %}
{% assign color = 8 | minus: base %}
{% endif %}
<a href="{{ root_url }}/{{ site.tag_dir }}#{{ tag[0] }}" style="font-size: {{ size }}pt; color: #{{ color }}{{ color }}{{ color }};">{{ tag[0] }}</a>
{% endfor %}
{% endraw %}
```
- 다시 `site.tags`를 대상으로 for루프를 돈다.
- 결국 필요한 값은 `size`, `color`이다.
- `size`, `color`를 계산하는데 필요한 값.
  - `temp` = ((`포스트 갯수`- `min`) * 36) / `diff`
  - `base` = `temp` / 4
  - `remain` = `temp` % 4
- `size` 설정
  - `remain`가 0일 때
    - `size` = `base` + 9
  - `remain`가 1 또는 2일 때
    - `size` = (`base` + 9).5
  - 그 외 나머지
    - `size` = `base` + 10
- `color` 설정 
  - `remain`가 0 또는 1일 때
    - `color` = 9 - `base`
  - 그 외 나머지
    - `color` = 8 - `base`
     
한번에 이게 뭘 하는 코드인지 단번에 알아보긴 힘들었다.
`size`값 더 분석해보면
- 먼저 `remain` 값에 따라 분기된다. 
- `remain`은 4로 나눈 나머지 값이기 때문에 0,1,2,3 네가지 중 하나로 나올 것이다.
- 그리고 1,2일 경우는 한대 묶고 세 갈래로 나누었다. 
- 그래서 `size`는 `base`+9, (`base`+9).5, `base`+10 세가지로 나뉜다.
- 이 부분은 글자 크기가 정수나 .5로 떨어지게 만드려는 목적으로 보인다. 
  
base를 계산하는 공식이 의미하는 것은 무엇인가? 

만약 `max`가 10000, `min`이 1일 경우 가장 큰 `base`값은 ((10000 - 1) * 36) / 9999 이므로 36이 된다.
`max`와 `min`이 아무리 차이나도 `temp`의 글자 크기는 0~36으로 제한될 수 있게 된다.
그럼 `base`는 0~9의 범위의 값이 되고 `size`는 0~19로 범위의 값이 된다.

마지막으로 `color`값을 살펴보자.
`remain`가 0 또는 1일 때 9 - `base`이고 나머지는 8 - `base`이다.
두 분류로 나눠서 글자색 차이를 두려는 게 목적인 듯 하다.
가져온 코드에는 포스트수가 많을 수록 검은색(#000)에 가깝게 되어있는데, 내 테마에는 맞지 않는다.

컬러값이 반전되도록 수정해야 한다.
컬러값이 15을 넘지 않도록 5, 6을 더해준다.
```
{% raw %}
{% if remain == 0 or remain == 1 %}
{% assign color = 5 | plus: base %}
{% else %}
{% assign color = 6 | plus: base %}
{% endif %}
{% endraw %}
```
 
10이상의 숫자는 알파벳으로 변환해준다.
```
{% raw %}
{% assign arr = "a, b, c, d, e, f" | split: ", " %}
{% if color > 9 %}
{% assign index = color | minus: 10 %}
{% assign color = arr[index] %}
{% endif %}
{% endraw %}
```

사이드바는 완성됐다.
![사이드바 완성]({{ site.url }}/assets/jekyll-category-005.png)

이어서 다음 포스트에는 사이드바에서 이동된 페이지를 구성해보겠다.
