## VMが大きくなって辛いよ

モーダルの表示フラグ・フラグの操作・コンテンツの入れ替えが1セット。
気づいたら何セットもあって、メイン部分が把握しづらくなってく。

```javascript
var vm = new Vue({
  data: {
    show: false,
    modalContent: null
  },
  methods: {
    openModal() {
      this.show = true
    }
    closeModal() {
      this.show = false
    }
    setModalContent() {
      ...
    }
  }
})
```

## Messenger パターン

```javascript
var vm = new Vue({
  methods: {
    openModal() {
      this.$emit('open-modal', (result) => {
        // コールバック
      })
    }
  }
})
```

```html
<div v-modal></div>
```

```javascript
Vue.directive('modal', () => {
  this.vm.$on('open-modal', (callback) => {
    var modal = new Modal()  // extended Vue
    modal.callback = callback

    var modalElement = document.createElement()
    this.el.appendChild(modalElement)
    modal.$mount(modalElement)
  })
})
```

先の問題だった、1セットが不要になってる。


## 疑問

* vm.$destroy をどこに入れようか？
* component ではなくて directive なのか？
* iOS の delegate　っぽい？
* $emit のイベントは同じインスタンスのみで親子間は？
  * $broadcast, $dispatch は 2.0 で deprecated
  * Vuex で書くには？


Source: http://kitak.hatenablog.jp/entry/2016/06/04/131414
