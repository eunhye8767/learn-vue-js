# LV.01 Vue.js 시작하기 - Age of Vue.js

##  1. vue.js 소개

### 1.1. MVVM 모델에서의 Vue
- Vue : MVVM 패턴의 뷰모델(ViewModel) 레이어에 해당하는 화면(View)단 라이브러리
![mvvm-vue](./_images/1-1-img1.png)
- View : (html => Dom) 사용자 화면
- Vue :
    1. Dom Listeners : 사용자 키보드 입력, 클릭 등의 정보
    2. Data Bindings : 서버에 저장된 정보를 사용자 화면에 출력

<br />

### 1.2. 기존 웹 개발 방식(HTML, Javascript)
- 기존 HTML 웹개발 방식 : HTML, CSS, Javascript
- playground/web-dev.html
    ```
    <div id="app"></div>
    <script>
        // console.log(div);
        var div = document.querySelector('#app');
        var str = 'hello word';
        div.innerHTML = str;
        
        // 일반적인 기본 개발 방식
        // 기존 코드 값 변경시 그 값을 다시 재실행
        str = 'hello word!!'
        div.innerHTML = str;
    </script>
    ```

<br />

### 1.3. Reactivity 구현
- html 문서에 아래 내용 추가
```
    <div id="app"></div>

    <script>
        var div = document.querySelector('#app');
        var viewModel = {};

        // 객체의 동작을 재정의 하는 api
        // Object.defineProperty(대상객체, 객체의 속성, {
        //    정의할 내용
        //})

        Object.defineProperty(viewModel,'str', {
            // 속성에 접근했을 때의 동작을 정의
            get: function() {
                console.log('접근');
            },
            // 속성에 값을 할당했을 때의 동작을 정의
            set: function(newValue) {
                console.log('할당', newValue);
                div.innerHTML = newValue;
            }
        })

    </script>
```
- 브라우저 console 탭에서 viewModel.str = 'hi' 입력
- 브라우저 뷰화면에서 hi 라는 문구가 보여지고 console 창엔 할당, hi 로 보여진다.
```
    <div id="app">hi</div>
```
- vue의 핵심은 데이터의 변화를 라이브러리에서 감지해서 알아서 화면을 자동으로 그려주는 것이 바로 Reactivity 라고 보면 된다.
- MVVM 모델에서의 Vue 에서 Data Bindings 을 브라우저 화면에서 보여지는 hi 라고 이해하면 된다.

<br />

### 1.4. Hello Vue.js 와 뷰 개발자 도구
- 뷰 개발자 도구 : 크롬에서 확장 프로그램 설치
    - https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd
    - 확장 프로그램을 설치하고 브라우저 재실행 후 개발자도구를 보면 vue 탭메뉴가 생성된다.
- vue 개발자 도구에서 컴포넌트 탭 > <Root> 클릭하면 Data 부분이 보여진다
- message 부분의 텍스트를 수정하면 브라우저에서 보여지는 부분이 바뀌는 것을 알 수 있다
- 이 기능이 Reactivity 기능이고 vue의 데이터 속성에 기본 기능이라고 이해하면 된다

<br /><br /><br />

##  2. 인스턴스

### 2.1.1 인스턴스 소개 - 뷰 인스턴스
- 인스턴스는 뷰로 개발할 때 **필수로 생성해야 하는 코드**

### 2.1.2 인스턴스 소개 - 인스턴스 생성 -1
- 인스턴스는 아래와 같이 생성할 수 있다
```
    new Vue();
```
- 인스턴스를 생성하고 나면 아래와 같이 인스턴스 안에 어떤 속성과 API가 있는 지 콘솔 창에서 확인할 수 있다
```
    var vm = new Vue();
    console.log(vm);
```
- 브라우저 console 탭에서 vm 을 입력하면 vue에 관한 인스턴스를 확인할 수 있다
```
    // 브라우저 Console 탭, clear console (ctrl + L)
    vm
```
<details><summary>Vue {_uid: 0, _isVue: true, $options: {…}, _renderProxy: Proxy, _self: Vue, …}</summary>
$attrs: (...)
$children: []
$createElement: ƒ (a, b, c, d)
$listeners: (...)
$options: {components: {…}, directives: {…}, filters: {…}, _base: ƒ}
$parent: undefined
$refs: {}
$root: Vue {_uid: 0, _isVue: true, $options: {…}, _renderProxy: Proxy, _self: Vue, …}
$scopedSlots: {}
$slots: {}
$vnode: undefined
_c: ƒ (a, b, c, d)
_data: {__ob__: Observer}
_directInactive: false
_events: {}
_hasHookEvent: false
_inactive: null
_isBeingDestroyed: false
_isDestroyed: false
_isMounted: false
_isVue: true
_renderProxy: Proxy {_uid: 0, _isVue: true, $options: {…}, _renderProxy: Proxy, _self: Vue, …}
_self: Vue {_uid: 0, _isVue: true, $options: {…}, _renderProxy: Proxy, _self: Vue, …}
_staticTrees: null
_uid: 0
_vnode: null
_watcher: null
_watchers: []
$data: (...)
$isServer: (...)
$props: (...)
$ssrContext: (...)
get $attrs: ƒ reactiveGetter()
set $attrs: ƒ reactiveSetter(newVal)
get $listeners: ƒ reactiveGetter()
set $listeners: ƒ reactiveSetter(newVal)
__proto__: Object
</details>

### 2.1.3 인스턴스 소개 - 인스턴스 생성 -2
- body 태그 안에서 app 이라고 아이디를 가진 태그를 찾아서 인스턴스를 붙이겠다는 의미.
- 붙이는 순간 vue의 기능과 속성들이 유효해진다. 
- el(엘리먼트)를 반드시 지정해줘야 사용할 수가 있다.
- 아래처럼 코드를 입력 후, 브라우저 vue 탭 > Root > data에서 입력한 정보를 확인할 수 있다
```
    <div id="app"></div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                message: 'hi'
            }
        });
    </script>
```

<br />

### 2.2.1 인스턴스와 생성자 함수 -1 javascript
- 기본적으로 자바스크립에서 어떤 함수를 이용하여 인스턴스를 생성하는 방법은 바로 생성자 함수를 이용하는 것이다.
```
    // Person 이라는 생성자 함수를 만들었다
    // 생성자 함수는 이름을 대문자로 시작한다
    function Person(name, job) {
        this.name = name;
        this.job = job;
    }

    new Person('josh', 'developer');
```
- 그러면 객체가 하나가 생성이 된다
```
    Person { name: josh, job: developer}
```
- 새로 생성된 객체를 P라는 객체 안에 넣어준다
```
    function Person(name, job) {
        this.name = name;
        this.job = job;
    }

    var p = new Person('josh', 'developer');

    console.log(p);
```
- p 객체에 대한 정보를 알 수 있다
```
    Person { name : "josh", job: "developer" }
```
- 이것이 기본 생성자 함수에 대한 정의라고 이해하면 된다

### 2.2.2 인스턴스와 생성자 함수 -2 Vue
- 생성자 함수를 이용해 logoText 라는 함수를 생성했다
```
    // function 에 Vue 라는 생성자 함수를 이용해서
    // 어떤 기능과 속성들을 사람들이 편하게 쓰게 하고 싶을 때
    // logText 함수를 미리 정의하려고 한다

    function Vue() {
        this.logText = function() {
            console.log('Hello');
        }
    }
```
- vm 으로 vue를 생성할 때 마다 객체 안에 logoText 란 함수가 들어가 있다<br />
따라서, 매번 함수를 정의하는 것이 아니라 미리 정의된 함수를 갖다가 사용할 수 있다<br />
이렇기 때문에 생성자 함수로 Vue 에서 api와 속성들을 다 정해놓고<br />
갖다 쓰거나 재사용하게 되는 패턴을 갖고 있는 것이다<br />
**이것이 new Vue()를 쓰는 이유다.**
```
    var vm = new Vue();

    vm.logoText();
```

- MDN 생성자 함수 설명 문서 :<br />
https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Obsolete_Pages/Core_JavaScript_1.5_Guide/Creating_New_Objects/Using_a_Constructor_Function

<br />

### 2.3 인스턴스 옵션 속성
- 인스턴스에서 사용할 수 있는 속성과 API는 다음과 같다
    1. el : 인스턴스가 그려지는 화면의 시작점 (특정 HTML 태그)
    2. template: 화면에 표시할 요소(HTML, CSS 등)
    3. data: 뷰의 반응성(Reactivity)가 반영된 데이터 속성
    4. methods: 화면의 동작과 이벤트 로직을 제어하는 메서드
    5. created: 뷰의 라이프 사이클과 관련된 속성
    6. watch: data에서 정의한 속성이 변화했을 때 추가 동작을 수행할 수 있게 정의하는 속성
```
    new Vue({
        // key : value 형태
        el: ,
        template: ,
        data: , 
        methods: ,
        created: ,
        watch: ,
    });
```
- 객체를 생성하고 그 안에 표기하는 vue 문법
```
    // var vm = new Vue({객체 속성값 적용});
    
    var vm = new Vue({
        el: '#app',
        data: {
            message: 'hi',
        },
        methods: {

        },
        created: function() {

        }
    });
```

<br /><br /><br />

##  3. 컴포넌트

### 3.1 컴포넌트 소개 :: 뷰 컴포넌트
- 컴포넌트는 **화면의 영역을 구분**하여 개발할 수 있는 뷰의 기능<br />
컴포넌트 기반으로 화면을 개발하게 되면 **코드의 재사용성이 올라가고 빠르게 화면을 제작**할 수 있다<br />

<br />

### 3.2 전역 컴포넌트 등록
- 인스턴스 생성 > - el: '#app' - 객체 생성
- div id="app"에 인스턴스를 붙이겠다, 적용하겠다 
```
    <div id="app"></div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        
        new Vue({
            el: '#app'
        })
    </script>
```
- Vue 인스턴스를 생성하면 기본적으로 Root 안에 컴포넌트가 된다<br />
![mvvm-vue](./_images/3-2-img1.png)<br />

- **전역 컴포넌트 등록하는 방법**
    1. component()로 전역 컴포넌트 등록
    ```
        Vue.Component('컴포넌트 이름', 컴포넌트 내용);
    ```
    2. 컴포넌트 내용엔 객체를 열어서 속성: 속성값 형식으로 적용한다
    3. 컴포넌트 내용을 적고 app-header 컴포넌트를 등록한다
    ```
        Vue.component('app-header', {
            template: '<h1>Header</h1>'
        });

        Vue.component('app-content', {
            template: '<div>content</div>'
        })

    ```
    4. 등록한 컴포넌트의 태그를 el: #app 항목 안에 적용해준다
    5. 컴포넌트한 태그를 적어주면 컴포넌트 template에 적용된 내용이 나타나게 된다
    ```
        <div id="app">
            <app-header></app-header>
            <app-content></app-content>
        </div>
    ```
    6. 전역 컴포넌트 등록은 실제 사용하는 일은 거의 없다고 한다.
    7. 브라우저, Vue 개발자 도구에서 app-header, app-content가 등록된 것을 확인할 수 있다<br />
        - Root = 상위 컴포넌트, 일반적으로 부모에 해당 된다
        - header, content 가 하위 컴포넌트로써 보통 자식 컴포넌트라고 부르기도 한다
    
![mvvm-vue](./_images/3-2-img2.png)

<br />

### 3.3 지역 컴포넌트 등록
- 서비스를 만들 때 가장 많이 쓰는 컴포넌트 등록 방법이다
- **지역 컴포넌트 등록하는 방법**
    1. 인스턴스 객체 안에 component 객체를 만들어준다 
        - {} 객체표기법(=객체 리터럴) ex) 속성명: {}
    ```
        new Vue({
            el: '#app',
            components: {
            }
        });
    ```
    2. component 에 생성해야 할 컴포넌트 객체를 만든다 ex) app-footer
        - 컴포넌트 내용을 적용해줄 때도 객체표기법('{}')을 이용해 작성한다
    ```
        new Vue({
            el: '#app',
            components: {
                // '키(속성)': '값(속성)',
                // '컴포넌트 이름' : 컴포넌트 내용
                'app-footer': {
                    template: '<footer>footer</footer>'
                }
            }
        });
    ```

    ![지역 컴포넌트 등록](./_images/3-3-img1.png)