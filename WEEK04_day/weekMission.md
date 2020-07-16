# TIL

UI Interaction Senior, 2020 TIL (Today I Learned)

## 4주차

<details open>

<summary>MISSION</summary>

### Toast 컴포넌트 ES6 Class로 바꿔보기

#### Toast.js

```js
((Y = {}) => {
  console.log(Y);
  Y.loadStyle = (source, callback) => {
    let s = null;
    if (source && typeof source === 'string') {
      s = document.createElement('link');
      s.setAttribute('href', source);
      s.setAttribute('rel', 'stylesheet');
      document.head.appendChild(s);
    }

    if (s && typeof callback === 'function') {
      s.addEventListener('load', callback);
    }
  };
  Y.loadScript = (source, callback) => {
    let s = null;
    if (source && typeof source === 'string') {
      s = document.createElement('script');
      s.setAttribute('src', source);
      document.body.appendChild(s);
    }

    if (s && typeof callback === 'function') {
      s.addEventListener('load', callback);
    }
  };

  Y.loadScripts = (list, callback) => {
    const _list = [...list].map(item => ({ url: item, complete: false }));

    _list.forEach(({ url, complete }) => {
      loadScript(url, function () {
        complete = true;
      });
    });

    let clearId = window.setInterval(() => {
      const allCompleted = _list.every(({ complete }) => complete === true);
      if (allCompleted) {
        window.clearInterval(clearId);
        typeof callback === 'function' && callback();
      }
    });
  };

  // 컴포넌트 스타일
  Y.loadStyle('./ui/Toast/Toast.css');

  /**
   * @class Toast
   */

  class Toast {
    /**
     * 생성자
     * @constructor
     * @param {Object} options 사용자 정의 옵션
     */

    constructor(options = {}) {
      // 옵션 객체 합성
      this._config = Object.assign({}, Toast.defaultOption, options);

      // DOM 노드
      this._toastZoneNode = null;
      this._toastNode = null;

      // 토스트 생성 (초기화)
      this.create();
    }

    /**
     * 컴포넌트 기본 옵션 (속성)
     * @static
     * @property
     * @memberof Toast
     */
    static defaultOption = {
      // 초기 표시 여부
      initVisible: true,
      // 자동 감춤 처리
      // ⚠️ 자동으로 감출 경우, 접근성 문제 야기하므로 주의
      autoHide: false,
      // 자동 감춤 처리 시간
      autoHideTime: 3000,
      // Toast 바디(body) 사용자 정의 마크업 콘텐츠
      html: 'Toast 컴포넌트 내용',
      // Toast에 추가할 클래스 속성 이름
      classes: '',
      // Toast 표시 지연 시간
      delay: 100,
      // Toast가 나타나면 콜백되는 함수
      onOpen: null,
      // Toast가 사라진 후 콜백되는 함수
      onClose: null,
    };

    /**
     * Toast 템플릿(Template)
     * @static
     */

    static template = [
      `<div class="toast">
      <div class="toast__content"></div>
      <button type="button" class="toast__closeButton" aria-label="닫기">×</button>
     </div>`,
    ];

    /**
     * Toast 컴포넌트 초기화 메서드
     * @memberof Toast.prototype
     */

    create() {
      // 문서에서 <div id="toastZone"></div> 요소를 찾아 참조
      this._toastZoneNode = document.querySelector('#toastZone');

      // <div id="toastZone"></div> 요소가 존재할 경우
      if (this._toastZoneNode) {
        // #toastZone 요소 영역의 뒷부분에 Toast.template 템플릿 HTML 코드를 추가
        // ⚠️ insertAdjacentHTML() 메서드 - IE 9+
        this._toastZoneNode.insertAdjacentHTML('beforeend', Toast.template);

        // #toastZone 안에서 .toast 요소들을 찾아 참조
        var toastNodeList = this._toastZoneNode.querySelectorAll('.toast');
        // Toast 인스턴스의 _toastNode에 마지막 .toast 요소노드 참조
        this._toastNode = toastNodeList[toastNodeList.length - 1];
        // .toast__content 요소 노드의 HTML 코드로 html 옵션 설정
        this._toastNode.querySelector(
          '.toast__content'
        ).innerHTML = this._config.html;
        // .toast__closeButton 요소 노드 이벤트 연결: Toast 닫기
        this._toastNode
          .querySelector('.toast__closeButton')
          .addEventListener('click', this.close.bind(this));

        // 초기 표시(init visible) 옵션 값이 설정된 경우, Toast 표시(열기)
        this._config.initVisible && this.open();
        // 클래스 이름 옵션이 추가된 경우, 클래스 이름 추가
        this._config.classes &&
          this._toastNode.classList.add(this._config.classes);
      } else {
        // 경고 메시지 출력
        console.warn(
          '경고!\n문서에 <div id="toastZone"></div> 요소가 존재하지 않습니다.\n해당 요소가 존재하는지 확인 후 다시 시도해보세요.'
        );
      }
    }

    /**
     * Toast 컴포넌트 화면에 표시하는 메서드
     * @memberof Toast.prototype
     */

    open() {
      // Toast {} 인스턴스 옵션 참조
      const _config = this._config;
      // Toast {} 인스턴스의 _toastNode 참조
      const _toastNode = this._toastNode;
      // 지연(delay) 옵션 뒤에 Toast 표시
      window.setTimeout(() => {
        _toastNode.classList.add('open');
        // onOpen 옵션이 함수로 설정된 경우, 콜백 실행
        typeof _config.onOpen === 'function' && _config.onOpen();
      }, _config.delay);

      console.log('config', _config);
      // 자동 감춤(auto hide) 설정된 경우
      if (_config.autoHide) {
        // 자동 감춤 시간 + 지연 시감 뒤에 Toast 닫기 실행
        window.setTimeout(() => {
          this.close();
        }, _config.autoHideTime + _config.delay);
      }
    }

    /**
     * Toast 컴포넌트 화면에서 감추는 메서드
     * @memberof Toast.prototype
     */
    close() {
      // Toast {} 인스턴스 옵션 참조
      const _config = this._config;
      // Toast {} 인스턴스의 _toastNode 참조
      const _toastNode = this._toastNode;
      _toastNode.classList.remove('open');

      window.setTimeout(() => {
        this.destory();
        typeof onClose === 'function' && _config.onClose();
      }, 500);
    }

    /**
     * Toast 컴포넌트 파괴(제거) 메서드
     * @memberof Toast.prototype
     */
    destory() {
      const _toastNode = this._toastNode;
      console.log(_toastNode);
      console.log(_toastNode.parentNode);
      if (_toastNode.parentNode) {
        _toastNode.parentNode.removeChild(_toastNode);
      }
    }
  }

  /**
   * Toast 생성 유틸리티 함수
   * @memberof Y
   * @static
   * @function
   */

  Y.toast = options => {
    // 함수가 실행되면 새로운 Toast 객체 생성 후, 반환
    return new Toast(options);
  };

  /**
   * Toast 리스트 생성 유틸리티 함수
   * @memberof Y
   * @static
   * @function
   */

  Y.toastList = optionsList => {
    // optionsList 순환하여 새로운 배열 반환
    return optionsList.map(options => {
      // 새로운 Toast 객체 생성 후, 반환
      return new Toast(options);
    });
  };

  Y.Toast = Toast;
})((window.Y = window.Y || {}));
```

코드 작성하면서 생긴 문제점

Uncaught TypeError: Cannot read property 'removeChild' of null at ToastComponent.Toast.destroy 콘솔 에러 발생

    해결: 조건문을 사용하여 \_toastNode.length 가 존재할 경우에만 destory() 메서드 실행

```js
    close() {
          // Toast {} 인스턴스 옵션 참조
          const _config = this._config;
          // Toast {} 인스턴스의 _toastNode 참조
          const _toastNode = this._toastNode;
          _toastNode.classList.remove('open');

          window.setTimeout(() => {
            if (_toastNode.length) {
              this.destory();
            }

            typeof onClose === 'function' && _config.onClose();
          }, 500);
        }

```

</details>
```
