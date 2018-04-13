---
layout: post
title: "CSS specificity"
author: Degan
categories: CSS
tags:	css specificity rule
comments: true
---

### CSS 규칙 간의 충돌이 생길 경우, 경쟁을 통해 우선순위가 정해진다

- inline > internal, external

- 선택자에 있는 ID 속성에 있는 갯수를 계산 (=a)
- 선택자에 있는 기타 다른 속성들과 pseudo-class들의 갯수를 계산 (=b)
- 선택자에 있는 요소 이름들의 갯수를 계산 (=c)
- pseudo-element들은 무시

```CSS
*              {}  /* a=0 b=0 c=0 -> specificity =   0 */
li             {}  /* a=0 b=0 c=1 -> specificity =   1 */
ul li          {}  /* a=0 b=0 c=2 -> specificity =   2 */
ul li a        {}  /* a=0 b=0 c=3 -> specificity =   3 */
a:hover        {}  /* a=0 b=1 c=1 -> specificity =  11 */
ul li a.red    {}  /* a=0 b=1 c=3 -> specificity =  13 */
li a.red:hover {}  /* a=0 b=2 c=2 -> specificity =  22 */
#candy         {}  /* a=1 b=0 c=0 -> specificity = 100 */
```

---

## Reference

[미남이의 이러쿵저러쿵](https://appletree.or.kr/blog/web-development/css/css-specificity/)
