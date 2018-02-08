---
title: 루프 돌며 이벤트 리스너 추가하기
layout: post
categories: [javascript]
tags: [javascript, functional-javascript]
---

[함수형 자바스크립트](http://www.insightbook.co.kr/book/programming-insight/%ED%95%A8%EC%88%98%ED%98%95-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D) 책을 보다가 예전에 코딩하며 문제가 됐던 내용이 생각났다.

배열이 있고, 배열 요소 각각의 데이터를 사용해서 이벤트 리스너를 추가할 때 발생할 수 있는 실수이다.

아래 코드는 내가 주로 사용했던 방법으로 이벤트 리스너를 추가한 것이다.
<p data-height="265" data-theme-id="0" data-slug-hash="wyoNaJ" data-default-tab="js,result" data-user="hoisharka" data-embed-version="2" data-pen-title="배열 요소를 루프 돌며 이벤트 추가하기(for문 사용)" class="codepen">See the Pen <a href="https://codepen.io/hoisharka/pen/wyoNaJ/">배열 요소를 루프 돌며 이벤트 추가하기(for문 사용)</a> by hoisharka (<a href="https://codepen.io/hoisharka">@hoisharka</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

for문을 보면 안쪽 코드가 즉시 실행되는 익명함수로 감싸져있다. 보통 아래와 같이 코딩해서 실수가 발생한다. 아래 코드는 이벤트 리스너가 제대로 추가되지 않는다.

``` javascript
 for(var i=0; i<users.length; i++) {
    var user = users[i];
    var button = $('<button type="button" class="btn btn-primary">')
      .text(user.name);

    button.click(function() {
      if (confirm(user.name + "님을팔로잉하시겠습니까?")) {
        follow(user);
      }
    });

    $('.user-list').append(button);
  }
 ```

이유는 button.click에 파라미터로 들어간 함수는 user와 같은 함수 스코프에서 선언되었고, for루프를 돌며 이벤트 리스너를 여러개 등록했지만 모두 하나의 user를 참조하게 된다. 그래서 등록한 이벤트 리스너 모두가 마지막으로 할당된 user의 값을 참조할 것이다.

이 문제를 해결하기 위해 즉시 실행하는 익명함수로 각각의 스코프 안에 user를 참조하게 했었다.

문제는 해결 됐지만 비슷한 코드를 작성할 때 알면서도 실수할 때가 있다. 이내 실수한 걸 알고 고치긴 하지만 실수할 여지를 만들지 않는다면 더 좋을 것이다.

이 책에서 그 방법이 소개되어 있었다. 바로 map함수를 사용해서 처리하는 것이다. 책에선 underscore.js의 _.map함수를 사용했는데, 자바스크립트 내장 함수인 map을 사용해도 동일한 효과를 낼 수 있을 것이다. (*map함수를 지원하지 않는 브라우저도 있다.*)

map함수는 파라미터를 2개 받는데, 첫번째는 배열이고 두번째는 배열 요소 각각을 파라미터로 받아 처리할 함수이다. map 함수 내부에서 이 함수를 이용해 배열 요소 각각에 대한 스코프를 만들기 때문에 for문에서 익명함수로 스코프를 만든 것과 비슷한 효과를 낼 수 있으며, for문을 사용했을 때처럼 실수할 일이 없다.

아래는 _.map을 사용한 예제이다. 책에 나온 내용과 거의 동일하다.

<p data-height="265" data-theme-id="0" data-slug-hash="eVBbeG" data-default-tab="js,result" data-user="hoisharka" data-embed-version="2" data-pen-title="배열 요소를 루프 돌며 이벤트 추가하기(map사용)" class="codepen">See the Pen <a href="https://codepen.io/hoisharka/pen/eVBbeG/">배열 요소를 루프 돌며 이벤트 추가하기(map사용)</a> by hoisharka (<a href="https://codepen.io/hoisharka">@hoisharka</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

* 참고
  - [함수형 자바스크립트](http://www.insightbook.co.kr/book/programming-insight/%ED%95%A8%EC%88%98%ED%98%95-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)
