---
layout: post
title: youtube api로 음악 재생 목록 가져오기
tag: [open-api, youtube] 
---

youtube로 음악 데이터를 가져와보자.

## youtube api key 받기
api 호출을 위해선 api key가 필요하고 [YouTube Data API 페이지](https://developers.google.com/youtube/v3/getting-started) 내용대로 따라해서 쉽게 받을 수 있었다.

생활코딩의 강좌 중에서도 해당 내용을 설명하는 영상이 있어서 링크를 추가한다.
- [https://youtu.be/rHEAHmNkfaI](https://youtu.be/rHEAHmNkfaI)

## API 호출
가져올 음악 목록이다. [음악 채널 > 인기트랙 - 한국](https://www.youtube.com/playlist?list=PLFgquLnL59alGJcdc0BEZJb2p7IgkL0Oe) 이고 URL의 끝에 보면 playlistId(`PLFgquLnL59alGJcdc0BEZJb2p7IgkL0Oe`)가 적혀 있다.
![playlist]({{ site.url }}/assets/youtube-music-001.png)

[youtube api 참조 문서](https://developers.google.com/youtube/v3/docs/playlistItems/list)에서 설명하는 `PlaylistItems: list`에 해당하는 API를 사용한다. 
![playlist doc]({{ site.url }}/assets/youtube-music-002.png)

최소한의 필요한 파라미터는 두가지이다.
  - part -> snippet이 적당하다.
  - playlistId -> 위에서 확인한 `PLFgquLnL59alGJcdc0BEZJb2p7IgkL0Oe`가 들어간다.
  
아래 URL에 API_KEY를 넣어 호출하면 된다. 
```
 https://www.googleapis.com/youtube/v3/playlistItems?playlistId=PLFgquLnL59alGJcdc0BEZJb2p7IgkL0Oe&part=snippet&maxResults=10&key={API_KEY}
```
![playlist result]({{ site.url }}/assets/youtube-music-003.png)
