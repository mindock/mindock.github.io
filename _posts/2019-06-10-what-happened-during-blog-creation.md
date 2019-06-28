---
published: true
layout: single
title: "블로그를 만들면서 생긴 일"
category: experience
tags: [error, blog, jekyll]
comments: false
---

## jekyll 개발환경에서 실행하는 방법

```
bundle exec jekyll serve
```

- http://localhost:4000 에 접속한다.

<https://jekyllrb-ko.github.io/docs/quickstart/>

## Error: Invalid CP949 character "\xE2" on line 54

- bundle exec jekyll serve 해당 명령어를 쳤더니 오류가 발생했다:(
  > windows에서 Jekyll 을 사용하는 것에 연관된 문제입니다.
  > 콘솔창의 코드 페이지를 UTF-8 로 변경해 문제를 해결한다.

```
chcp 65001
```

<http://jekyllrb-ko.github.io/docs/windows/#인코딩>
