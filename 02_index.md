## 2. [시작하기](https://kr.vuejs.org/v2/guide/index.html)
### 선언적 렌더링
* [JSFiddle로 연습해보자](https://jsfiddle.net/)
* Mustache를 이용한 표현식을 사용 가능
    ```
    <div id="app">
      {{message}}
    </div>
    
    <script>
      var app = new Vue({
        el: '#app',
        data: {message: 'Hello vue!'}
      })
    </script>
    ```
* v-bind를 통한 속성 바인딩
    ```
    <div id="app-2">
      <!-- span title="" 확인 -->
      <span v-bind:title="message">
        내 위에 잠시 마우스를 올리면 동적으로 바인딩 된 title을 볼 수 있습니다!
      </span>
    </div>
    
    <script>
      var app2 = new Vue({
        el: '#app-2',
        data: {
          message: '이 페이지는 ' + new Date() + ' 에 로드 되었습니다'
        }
      })
    </script>
    ```
    + v-bind 속성을 **디렉티브**라고 한다. vue는 디렉티브의 접두어를 v-로 사용한다.
    + Vue로 생성 된 app, app2에 **변수명.{{data 내의 키}}** 로 접근 시 data 키에 등록된 변수에 접근할 수 있다.
    
### 조건문과 반복문
* if문의 사용
    ```
    <div id="app-3">
      <!-- data.seen이 true이므로 p 엘리먼트가 렌더링된다 -->
      <p v-if="seen">이제 나를 볼 수 있어요</p>
    </div>
    
    <script>
      var app3 = new Vue({
        el: '#app-3',
        data: {
          seen: true
        }
      })
    </script>
    ```
* for문의 사용
    ```
    <div id="app-4">
      <ol>
        <!-- JS저럼 in을 사용 -->
        <li v-for="todo in todos">
          {{ todo.text }}
        </li>
      </ol>
    </div>
    
    <script>
      var app4 = new Vue({
        el: '#app-4',
        data: {
          todos: [
            { text: 'JavaScript 배우기' },
            { text: 'Vue 배우기' },
            { text: '무언가 멋진 것을 만들기' }
          ]
        }
      })
      //app4.todos.push({text: 'New Item'}) //을 수행할 경우 목록에 신규로 추가된다.
    </script>
    
### 사용자 입력 핸들링
* v-on을 사용한 이벤트 리스닝
    ```
    <div id="app-5">
      <p>{{ message }}</p>
      <button v-on:click="reverseMessage">메시지 뒤집기</button>
    </div>

    <script>
      var app5 = new Vue({
        el: '#app-5',
        data: {
          message: '안녕하세요! Vue.js!'
        },
        methods: {
          reverseMessage: function () {
            this.message = this.message.split('').reverse().join('')
          }
        }
      })
    </script>
    ```
* v-model을 사용한 양방향 제어
    ```
    <div id="app-6">
      <p>{{ message }}</p>
      <input v-model="message">
    </div>
    
    <script>
      var app6 = new Vue({
        el: '#app-6',
        data: {
          message: '안녕하세요 Vue!'
        }
      })
    <script>

    ```
    
### 컴포넌트의 사용
* 미리 정의된 옵션을 가진 Vue 인스턴스
* 컴포넌트 등록 방법
    ```
    //template 내의 문자열이 dom으로 변환
    Vue.component('todo-item', {template: '<li>할일 하나.</li>})
    
    //다른 컴포넌트에서 todo-item 컴포넌트를 사용
    <ol>
      <todo-item></todo-item>
    </ol>
    ```
* 컴포넌트에 데이터 주입 [props](https://kr.vuejs.org/v2/guide/components.html#%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EC%9E%91%EC%84%B1)
    ```
    Vue.component('todo-item', {
      //todo-item 컴포넌트 내에 todo라는 이름의 속성을 정의
      props: ['todo'],
      //todo로 전달받은 객체 내의 text를 바인딩
      template: '<li>{{ todo.text }}</li>'
    })
    
    //다른 컴포넌트에 v-bind를 이용하여 값을 주입할 수 있다.
    <div id="app-7">
      <ol>
        <!-- 
          groceryList를 반복하며 해당 item을 todo로 제공
          또한 각 구성 요소에 id값에 해당하는 "키"를 제공해야한다 (추후)
         -->
        <todo-item
          v-for="item in groceryList"
          v-bind:todo="item"
          v-bind:key="item.id">
        </todo-item>
      </ol>
    </div>
    
    <script>
      var app7 = new Vue({
        el: '#app-7',
        data: {
          groceryList: [
            { id: 0, text: 'Vegetables' },
            { id: 1, text: 'Cheese' },
            { id: 2, text: 'Whatever else humans are supposed to eat' }
          ]
        }
      })
    </script>
    ```
    + 이를 이용하여 화면 개발시 서로 다른 템플릿의 집합으로 구성할 수 있다. (재사용성+)
        ```
          <div id="app">
            <app-nav></app-nav>
            <app-view>
              <app-sidebar></app-sidebar>
              <app-content></app-content>
            </app-view>
           </div>
        ```
