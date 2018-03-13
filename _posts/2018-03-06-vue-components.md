---
layout: post
title: "Vue Components basic"
author: Degan
categories: vue
tags:	vue component components
comments: true
---

#### 컴포넌트란?
- 조합하여 화면을 구성할 수 있는 블록
- 화면을 빠르게 구조화하여 일괄적인 패턴으로 개발 가능

#### 지역 컴포넌트와 전역 컴포넌트의 차이

```html
/* index.html */

<div id="app">
    <h3>첫 번째 인스턴스 영역</h3>
    <my-global-component></my-global-component>
    <my-local-component></my-local-component>
</div>

<hr>

<div id="app2">
  	<h3>두 번째 인스턴스 영역</h3>
  	<my-global-component></my-global-component>
  	<my-local-component></my-local-component>
</div>
```

```javascript
// javascript

Vue.component('my-global-component', {
      template: '<div>전역 컴포넌트입니다.</div>'
    })

    var cmp = {
      template: '<div>지역 컴포넌트입니다.</div>'
    }

    new Vue({
      el: '#app',
      components: {
        'my-local-component': cmp
      }
    })

    new Vue({
      el:'#app2'
    })
```

- 결과

```
첫 번째 인스턴스 영역

전역 컴포넌트입니다.

지역 컴포넌트입니다.

----------

두 번째 인스턴스 영역

전역 컴포넌트입니다.
```

#### 뷰 컴포넌트 통신

- 앵귤러1이나 백본(Backbone.js)
  - 초창기 자바스크립트 프레임워크
  - 한 화면을 1개의 뷰(view)로 간주
  - 한 화면의 데이터를 해당 화면 어디서든지 호출 가능
- 뷰(vue.js)
  - 컴포넌트로 화면을 구성
  - 같은 웹 페이지라도 데이터를 공유할 수 없음
  - 각 컴포넌트의 유효 범위가 독립적이기 때문
- 상,하위 컴포넌트 관계
  - 상위에서 하위로는 props라는 특별한 속성을 전달
  - 하위에서 상위로는 기본적으로 이벤트만 전달
    - 이벤트와 함께 데이터를 전달하고 싶은 경우
    - 이벤트의 두 번째 인자 값으로 잔달 하거나
    - 이벤트 버스(Event Bus)를 활용
![상하관계](https://kr.vuejs.org/images/props-events.png)
    - 출처: https://kr.vuejs.org

```html
/* 상위에서 하위로 props 전달 */

  <div id="app">
    <child-component v-bind:propsdata="message"></child-component>
  </div>

  <script>
    Vue.component('child-component', {
      props: ['propsdata'],
      template: '<p>{{ propsdata }}</p>',
    });

    new Vue({
      el: '#app',
      data: {
        message: 'Hello Vue! passed from Parent Component'
      }
    })
  </script>
```

```html
/* 하위에서 상위로 이벤트 전달 */

  <div id="app">
    <child-component v-on:show-log="printText"></child-component>
  </div>

  <script>
    Vue.component('child-component', {
      template: '<button v-on:click="showLog">show</button>',
      methods: {
        showLog: function() {
          this.$emit('show-log');
        }
      }
    })

    var app = new Vue({
      el: '#app',
      methods: {
        printText: function() {
          console.log("received an event");
        }
      }
    })
  </script>
```

```html
/* 이벤트 버스 */

  <div id="app">
    <child-component></child-component>
  </div>

  <script>
    // 이벤트 버스 생성
    // 편하지만 컴포넌트가 많아지면 문제 발생
    // 이 문제를 해결하려면 뷰엑스(Vuex) 사용
    var eventBus = new Vue();

    Vue.component('child-component', {
      template: '<div>하위 컴포넌트 영역입니다.<button v-on:click="showLog">show</button></div>',
      methods: {
        showLog: function() {
          eventBus.$emit('triggerEventBus', 100);
        }
      }
    });

    var app = new Vue({
      el: '#app',
      created: function() {
        eventBus.$on('triggerEventBus', function(value) {
          console.log("이벤트를 전달받음. 전달받은 값: ", value);
        });
      }
    });

  </script>
```

---

## Reference


[Do it! Vue.js 입문](http://www.yes24.com/24/goods/58206961)
