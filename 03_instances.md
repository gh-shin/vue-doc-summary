## 3. [Vue 인스턴스](https://kr.vuejs.org/v2/guide/instance.html)
### Vue 인스턴스 만들기
* Vue 인스턴스는 new Vue({데이터,템플릿,메소드 etc...})의 형태로, options 객체를 포함하여 생성한다. [Vue API](https://kr.vuejs.org/v2/api/)
### 속성과 메소드
* 각 Vue 인스턴스는 data 내의 객체를 프록시 처리하며, 데이터가 변경될 경우 화면이 다시 렌더링
* Vue 인스턴스 생성 후 data 객체에 직접 신규로 생성하는 속성의 경우는 반응형으로 처리할 수 없다.
    ```
    //vueInstance: new Vue, data.a = 'hello'
    vueInstance.b = 'hi'
    //이와 같을 경우 객체 내부에 b 속성이 추가되지만 Vue에서 적용되지 않음
    ```
    + Vue 인스턴스 생성 시 빈 값 속성을 주입해야 한다.
        ```
        //vueInstance 생성 시
        data: {
          a: 'hello',
          b: 'hi'
        }
        ```
* Object.freeze()를 사용하는 경우에는 반응형으로 처리할 수 없다.
    ```
    <div id="app">
      <p>{{ foo }}</p>
      <!-- 상단 p 태그의 내용은 변하지 않는다. -->
      <button v-on:click="foo = 'baz'">Change it</button>
    </div>

    <script>
      var obj = {foo: 'bar'}
      Object.freeze(obj)    
      new Vue({el: '#app', data: obj})
    </script>
    ```

* Vue 인스턴스 생성 후 자체 속성 및 메소드를 사용할 수 있다. (접두어 $)
    ```
    var data = { a: 1 }
    var vm = new Vue({
      el: '#example',
      data: data
    })

    vm.$data === data // => true
    vm.$el === document.getElementById('example') // => true

    // $watch 는 Vue 객체의 인스턴스 메소드
    vm.$watch('a', function (newVal, oldVal) {
      // `vm.a`가 변경되면 호출
    })
    ```
### 인스턴스 라이프사이클 훅
* Vue 인스턴스의 생성 주기에 맞춰 리스너를 지정할 수 있다.
    ```
    new Vue({
      data: {
        a: 1
      },
      created: function () {
        // `this` 는 vm 인스턴스를 가리킵니다.
        console.log('a is: ' + this.a)
      }
    })
    // => "a is: 1"
    ```
* mounted, updated, destroyed 등이 있다.
* Vue 생성 시에 주입하는 options 내에 [화살표 함수식](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/%EC%95%A0%EB%A1%9C%EC%9A%B0_%ED%8E%91%EC%85%98) 사용을 지양할 것
  - ex) created: () => console.log(this.a)
  - ex) vm.$warch('a', newValue => this.myMethod())
  - this 사용 시에 오류가 발생할 수 있다.
### 라이프사이클 다이어그램
![라이프사이클 다이어그램](https://kr.vuejs.org/images/lifecycle.png)
***
[목록으로](https://github.com/gh-shin/vue-doc-summary)
