## 1. [계산된 속성과 감시자](https://kr.vuejs.org/v2/guide/computed.html)
### 계산된 속성
> 기본 표현식의 장황함을 피하기 위해 **계산된 속성** 을 사용한다.

* 기본 예제
    ```
    <div id="example">
      <!-- 안녕하세요 -->
      <p>원본 메시지: "{{ message }}"</p>
      <!-- 요세하녕안 -->
      <p>뒤집히도록 계산된 메시지: "{{ reversedMessage }}"</p>
    </div>

    <script>
    var vm = new Vue({
      el: '#example',
      data: {
        message: '안녕하세요'
      },
      computed: {
        // 계산된 getter
        reversedMessage: function () {
          // `this` 는 vm 인스턴스를 가리킵니다.
          return this.message.split('').reverse().join('')
        }
      }
    })
    </script>
    ```
* 계산된 캐싱 vs 메소드
    ```
    //상단 예제와 같은 결과
    <p>뒤집힌 메시지: "{{ reversedMessage() }}"</p>
    // 컴포넌트 내부
    methods: {
      reversedMessage: function () {
        return this.message.split('').reverse().join('')
      }
    }
    ```
    - computed의 경우 종속성 중 일부가 변경된 경우에만 다시 계산되며, 한 번 계산된 값을 캐시처리
    - computed의 경우 반응형 의존성을 가지지 않기 때문에 다음과 같은 코드는 업데이트 되지 않는다.
        ```
        computed: {
          now: function () {
            //한 번 계산 된 후, 종속성이 없어 변경점을 찾지 못해 처음 한 번만 수행
            return Date.now()
          }
        }
        ```
    - method의 경우 캐시처리하지 않고 매번 실행
* 계산된 속성 vs 감시된 속성
    ```
    <div id="demo">{{ fullName }}</div>
    ```
    - computed의 경우
        ```
        var vm = new Vue({
          el: '#demo',
          data: {
            firstName: 'Foo',
            lastName: 'Bar'
          },
          computed: {
            fullName: function () {
              return this.firstName + ' ' + this.lastName
            }
          }
        })
        ```
    - watch의 경우
        ```
        var vm = new Vue({
          el: '#demo',
          data: {
            firstName: 'Foo',
            lastName: 'Bar',
            fullName: 'Foo Bar'
          },
          watch: {
            firstName: function (val) {
              this.fullName = val + ' ' + this.lastName
            },
            lastName: function (val) {
              this.fullName = this.firstName + ' ' + val
            }
          }
        })
        ```
* 계산된 setter
    - computed의 경우 기본적으로 getter만 가지고 있지만, 필요한 경우 setter를 추가할 수 있다.
        ```
        // ...
        computed: {
          fullName: {
            // getter
            get: function () {
              return this.firstName + ' ' + this.lastName
            },
            // setter
            set: function (newValue) {
              var names = newValue.split(' ')
              this.firstName = names[0]
              this.lastName = names[names.length - 1]
            }
          }
        }
        // ...
        ```
        + vm.fullName = 'John Doe'를 실행하면 setter가 호출되어 해당 메서드를 수행한다. (내부 프록시가 set 메서드를 대리자로 실행하는듯)
### 감시자
> watch 옵션은 데이터 변경에 대한 응답으로, 비동기식, 혹은 시간이 많이 소요되는 조작을 수행할 경우 유용

* 기본 예제
    ```
    <div id="watch-example">
      <p>
        yes/no 질문을 물어보세요:
        <input v-model="question">
      </p>
      <p>{{ answer }}</p>
    </div>

    <!-- axios를 이용하여 ajax 수행 -->
    <script src="https://unpkg.com/axios@0.12.0/dist/axios.min.js"></script>
    <!-- lodash 라이브러리 import -->
    <script src="https://unpkg.com/lodash@4.13.1/lodash.min.js"></script>

    <script>
    var watchExampleVM = new Vue({
      el: '#watch-example',
      data: {
        question: '',
        answer: '질문을 하기 전까지는 대답할 수 없습니다.'
      },
      watch: {
        // 질문이 변경될 때 마다 이 기능이 실행
        question: function (newQuestion) {
          this.answer = '입력을 기다리는 중...'
          this.getAnswer()
        }
      },
      methods: {
        // _.debounce는 작업을 실행할 수 있는 빈도를 제한
        // _.debounce 함수(또는 이와 유사한 _.throttle)는 https://lodash.com/docs#debounce 를 참고
        getAnswer: _.debounce(
          function () {
            if (this.question.indexOf('?') === -1) {
              this.answer = '질문에는 일반적으로 물음표가 포함 됩니다. ;-)'
              return
            }
            this.answer = '생각중...'
            var vm = this
            axios.get('https://yesno.wtf/api')
              .then(function (response) {
                vm.answer = _.capitalize(response.data.answer)
              })
              .catch(function (error) {
                vm.answer = '에러! API 요청에 오류가 있습니다. ' + error
              })
          },
          // 사용자가 입력을 기다리는 시간(밀리세컨드)
          500
        )
      }
    })
    </script>
    ```
* watch 옵션을 사용하여 비동기 연산 + 수행 제한 + 최종 결과가 나오기 전까지 중간 상태를
* computed는 수행 불가(why?)
* [참고 vm.$watch](https://kr.vuejs.org/v2/api/#vm-watch)
***
[목록으로](https://github.com/gh-shin/vue-doc-summary)
