## 4. [템플릿 문법](https://kr.vuejs.org/v2/guide/syntax.html)
### 보간법
* 문자열
    - ```<span>메시지: {{msg}}</span>```
    - ```<span v-once>다시는 변경하지 않습니다: {{msg}}</span>```
    - v-once를 사용하는 경우 데이터 변경시 업데이트 되지 않으며, 동일 노드의 바인딩에도 영향을 미친다는 점을 유의해야 한다 (?)
* html
    - 실제 HTML을 이용하기 위해서는 v-html을 사용해야 함
    -   ```rawHtml = <span style="color:red">This should be red.</span>```의 예
      - Mustache. HTML 구문을 텍스트로 해석
          ```
          <p>Using mustaches: {{ rawHtml }}</p>
          //결과
          Using mustaches: <span style="color:red">This should be red.</span>
          ```
      - v-html. HTML 구문을 HTML로 해석
          ```
          <p>Using v-html: <span v-html="rawHtml"></span></p>
          //결과
          Using mustaches: This should be red.
          ```
    - XSS 취약점에 유의할 것
    - Mustache는 HTML 엘리먼트의 속성에서 사용할 수 없다. v-bind를 사용할 것
    - v-bind 값이 boolean일 경우, 대상 값이 **null, undefined, false** 일 경우 해당 속성이 렌더링 되지 않음
        ```
        var isEnabled=false

        <button v-bind:disabled="isEnabled">Button</button>
        //다음의 경우는 disabled 속성이 렌더링되지 않음
        ```
* JS 표현식 사용 (값이 리턴되는 표현식이나 삼항연산자를 사용해야 한다.)
    - ```{{number + 1}}```
    - ```{{ok ? 'YES' : 'NO'}}```
    - ```{{msg.split('').reverse().join('')}}```
    - ```<div v-bind:id="'list-' + id"></div>```
    - ```{{var a = 1}}``` -> *불가*
    - 템플릿 표현식은 샌드박스 처리되며 전역으로 사용 가능한 것에만 접근 가능 (Math, Date)
    - 사용자 정의 전역에 액세스하지 말 것 (use JS default)
### 디렉티브
> **v-** 접두사로 시작하는 특수 속성이며, 값은 단일 JS 표현식이 됨(v-for 제외)
>> ```<p v-if="seen">이제 나를 볼 수 있어요</p>``` --> p 엘리먼트를 제거 또는 삽입
* 전달 인자
    - **:** 로 전달인자를 사용할 수 있다.
      ```
      <a v-bind:href="url">...</a>
      <a v-on:click="method">...</a>
      ```
* 수식어
    - 점으로 표시되는 특수 접미사
        ```
        <form v-on:submit.prevent="onSubmit">...</form>
        ```
### 약어
* v-bind -> :로 대체
    ```
    <a v-bind:href="url">...</a>
    <a :href="url">...</a>
    ```
* b-on -> @로 대체
    ```
    <a v-on:click="method">...</a>
    <a @click="method>...</a>"
    ```
***
[목록으로](https://github.com/gh-shin/vue-doc-summary)
