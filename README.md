## GDSC Yonsei Tech Team Blog 필독 해주세요!!

#### 글 작성 방법
1. \_posts 폴더에서 카테고리명과 같은 이름으로 폴더를 하나 만듭니다.

2. '카테고리명' 폴더 안에 다음과 같은 이름으로 markdown 파일을 하나 만듭니다.
    * <날짜>-<제목>-<본인이름영어>.md
    * ex) 2022-01-19-FlutterStudy-leeryeong.md

3. 설정을 아래와 같이 합니다.
~~~
---
title: "제목" # 제목을 적어주세요
excerpt: "글 요약" # 요약을 적어주세요
author : LeeRyeong Song # 본인 프로필 이름을 입력해주세요. authors.yml에 적은 이름으로 적어주세요.
categories:
  - Flutter #카테고리명을 적어주세요. 카테고리는 1개만 적어야 하며, 띄어쓰기가 없어야 합니다.
tags:
  - 태그1 #태그를 적어주세요. 태그는 여러 개 적을 수 있으며, 띄어쓰기가 없어야 합니다.
  - 태그2
---
~~~


#### 사진 넣기
마크다운에 사진을 넣고 싶으면, 아래와 같이 해주세요.

1. /assets/ 현재 제목을 입력하고 이미지 파일을 넣어주세요.
2. 마크 다운에서 아래와 같은 명령어를 입력해주세요.

~~~
![image](/assets/<제목>/<이미지명>)
~~~

* Example
~~~
![image](/assets/OptimalPolicy/image1.png)
~~~


#### 마크다운 사용법
[이 링크를 참조하여 글을 작성해주세요](https://gist.github.com/danggai/7c2ba1c5e3f923e85411fb740bf514fa)

[수식은 이 링크를 참조하여 글을 작성해주세요](https://ko.wikipedia.org/wiki/%EC%9C%84%ED%82%A4%EB%B0%B1%EA%B3%BC:TeX_%EB%AC%B8%EB%B2%95)

## ISSUE
* 수식 중 "$$", "$$" 가 제대로 작동하지 않습니다. "$", "$"를 사용해주세요. 원인을 파악중입니다.
