---
layout: post
title: "CSS Normal Flow"
author: Degan
categories: CSS
tags:	css nomal flow bfc ifc rp
comments: true
---

[!nomal_flow](http://mooc.phinf.nhnnext.org/20171201_180/1512139974549QQl05_PNG/33.PNG)

### Normal flow
- 가장 기본이 되는 CSS 알고리즘
- position 속성이 static이나 relative일 때 위치를 결정
- BFC(Block Formatting Context)
	- 줄 단위의 위치를 계산하는 시스템
- IFC(Inline Formatting Context)
	- 한 줄을 가로로 채워가는 위치를 계산하는 시스템
- RP(Relative Positioning)
	- nomal flow 하에서 상대적인 위치를 처리하는 시스

---

[!bfc_ifc](http://mooc.phinf.nhnnext.org/20171201_250/1512140056477yDvoY_PNG/38.PNG)
	- 하나의 화면은 BFC와 IFC가 여러군데서 협조해서 그려짐

[!find_block](http://mooc.phinf.nhnnext.org/20171202_14/1512140752122wmmsU_PNG/47.PNG)
	- 블록요소의 크기는 부모를 계속 탐색하여 다른 블록요소의 크기를 찾음

[!rp](http://mooc.phinf.nhnnext.org/20171202_71/1512140844980leGjf_PNG/51.PNG)
	- IFC/BFC 관점에서 그리고 상대적인 위치를 줌
	 
---

## Reference

[CodeSpitz - CSS Rendering](http://www.edwith.org/codespitz-css-rendering)
