# Khái niệm cơ bản của Vue
## ToC
- [Template](#template)
  - [Cách bind dữ liệu từ javascript](#text-interpolation)
  - [Sử dụng rawHtml](#raw-html)
  - [Bind thuộc tính](#attribute-bindings)
  - [Thuộc tính logic](#boolean-attributes)
  - [Bind nhiều thuộc tính cùng lúc](#chủ-động-bind-nhiều-thuộc-tính-cùng-một-lúc)
  - [Directives](#directives)
  - [Arguments và Dynamic Arguments](#arguments)
  - [Modifiers](#modifiers)
- [State/method](#statemethod)
  - [Khởi tạo Reactive state](#khởi-tạo-một-reactive-state)
  - [Khởi tạo methods](#khởi-tạo-các-method)
  - [Deep Reactivity](#deep-reactivity)
- [Computed/watch](#computedwatch)
  - [computed](#lý-do-sử-dụng-computed)
    - [Writable Computed](#writable-computed)
    - [Watcher](#watchers)
  - [Deep Watcher](#deep-watchers)
    - [Eager Watcher](#eager-watchers)
    - [Callback Flush Timing](#callback-flush-timing)
    - ['this.$watch'](#thiswatch)
    - [Stopping Watcher](#stopping-a-watcher)
- [Template Refs](#template-refs)
  - [Truy cập vào các Ref](#truy-cập-vào-các-ref)
  - [Refs trong v-for](#refs-trong-vòng-lặp-v-for)
  - [Function Refs](#function-refs)
  - [Ref trong component](#ref-trong-một-component)
- [Class & Style binding](#class--style-binding)
- [Condition rendering](#condition-rendering)
- [List rendering](#list-rendering)
- [Event handling](#event-handling)
- [Form input binding](#form-input-binding)
- [Lifecycle hooks](#lifecycle-hooks)
## Template
Vue dùng một thẻ `<Template>` để chứa các component, các component này sau đó sẽ được Vue biên dịch thành các thẻ HTML.
### Text Interpolation
Dùng để bind dữ liệu từ javascript, sử dụng 2 dấu ngoặc nhọn `{{}}`:
```HTML
<span>Message: {{ msg }}</span>
```
Giá trị của `msg` sẽ được thay thế bằng giá trị của biến tương ứng, và có thể tự thay đổi khi giá trị của `msg` thay đổi.
### Raw HTML
Giá trị trong dấu `{{}}` khi được biên dịch sẽ trở thành chuỗi thông thường chứ không phải định dạng HTML. Để có thể in ra định dạng HTML, chúng ta sẽ sử dụng `v-html`:
```HTML
<p>Using text interpolation: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```
<pre>
Using text interpolation: &lt;span style=&quot;color: red&quot;&gt;This should be red.&lt;/span&gt;

Using v-html directive: <span style="color: red">This should be red.</span>
</pre>
Giá trị trong thẻ `<span>` sẽ luôn được thay thế với giá trị của `rawHtml`

> Chỉ nên sử dụng chỉ thị này khi in ra giá trị đáng tin cậy, không được dùng trong các nội dung mà người dùng cung cấp

### Attribute Bindings
Dấu `{{}}` không thể nằm trong Attribute của HTML. Mà thay vào đó, chúng ta sử dụng `v-bind`:
```html
<div v-bind:id="dynamicId"></div>
<!-- Shorthand -->
<div :id="dynamicId"></div>
```
`v-bind` sẽ hướng dẫn Vue giúp cho giá trị của thuộc tính `id` đồng bộ với giá trị của `dynamicId`. Nếu giá trị của nó là `null` hoặc `undefined`, thuộc tính sẽ bị xóa khi được render.
### Boolean Attributes
Là những thuộc tính có thể biểu thị giá trị true/false chỉ bằng hiện diện của nó ở element. Ví dụ, `disable` là thuộc tính logic được dùng nhiều nhất.
```html
<button :disabled="isButtonDisabled">Button</button>
```
`disable` sẽ được hiển thị nếu giá trị của `isButtonDisabled` thuộc dạng `truthy`, chuỗi rỗng `''` cũng được tính vào.
### Chủ động bind nhiều thuộc tính cùng một lúc
Có thể sử dụng `v-bind` không cần đối số
```javascript
data() {
  return {
    objectOfAttrs: {
      id: 'container',
      class: 'wrapper'
    }
  }
}
```
```html
<div v-bind="objectOfAttrs"></div>
```
Nó sẽ làm cho thẻ `div` có cả thuộc tính `id` và `class` với các giá trị trong `objectOfAttrs`
### Directives
Chỉ thị(Directives) là những thuộc tính đặc biệt có `v-` ở đằng trước.

Các thuộc tính directive thường có giá trị là javascript expressions (ngoại lệ là `v-for`, `v-on` và `v-slot`).

Công việc của các thuộc tính này sẽ là phản ứng để thay đổi DOM khi các giá trị trong chúng thay đổi, chẳng hạn:
```html
<p v-if="seen">Now you see me</p>
```
Ở đây thẻ `<p>` có thể được thêm vào hoặc loại bỏ tùy thuộc vào giá trị logic của `seen`
### Arguments
Một vài thuộc tính có thể có một đối số, được biểu diễn phía trước dấu `:`, lấy ví dụ `v-bind`.
```html
<a v-bind:href="url"> ... </a>
<!-- shorthand -->
<a :href="url"> ... </a>
```
Mọi đối số khi có `v-bind` đứng trước, chẳng hạn `v-bind:id` có thể viết tắt thành `:id`.

Một ví dụ viết tắt khác, đó là `v-on`, có thể dùng để lắng nghe sự kiện của DOM có thể viết tắt bằng dấu `@`.
### Dynamic Arguments
Có thể dùng javascript expressions để biểu diễn một đối số trong Vue bằng cách bọc chúng lại trong dấu `[]`.
```html
<a v-bind:[attributeName]="url"> ... </a>
<!-- shorthand -->
<a :[attributeName]="url"> ... </a>
```
Ở đây, `attributeName` sẽ được chủ động đổi sang giá trị mà nó được gán, giả sử `'href'` thì giá trị cuối cùng của thuộc tính sẽ là `v-bind:href`.
### Modifiers
Là một hậu tố được xác định bởi `.`, nó sẽ làm cho directive ràng buộc với một hành động đặc biệt nào đó. Ví dụ, `.prevent` sẽ làm cho `v-on` gọi `event.preventDefault()` khi sự kiện được kích hoạt:
```html
<form @submit.prevent="onSubmit">...</form>
```
Một directive đầy đủ sẽ có dạng như thế này:
![](https://vuejs.org/assets/directive.69c37117.png)
## State/method
### Khởi tạo một Reactive state
Với `Option API`, chúng ta dùng `data` để khởi tạo Reactive state của một component, các giá trị sẽ được `return` thông qua một object. Các giá trị này có thể được gọi bởi các hàm cấp cao thông qua toán tử `this` hoặc các lifecycle hook.
```javascript
export default {
  data() {
    return {
      count: 1
    }
  },
  mounted() {
    // `this` refers to the component instance.
    console.log(this.count) // => 1

    // data can be mutated as well
    this.count = 2
  }
}
```
Các giá trị này có thể được gọi khi và chỉ khi chúng đã được trả về ở hàm `data()`. Nếu giá trị chưa biết lúc khởi tạo, chúng ta có thể sử dụng `null` hoặc `undefined`.   

Có thể khởi tạo một giá trị bên ngoài hàm data. Tuy nhiên, chúng sẽ không được tính như là Reactive state.   

Vue dùng các tiền tố `$` và `_` để định nghĩa các hàm built-in, cho nên tránh sử dụng các tiền tố này cho việc định nghĩa.
### Khởi tạo các Method
Để tạo các method cho component, chúng ta dùng `methods`. Đây là một object chứa tất cả những method mong muốn.
```javascript
export default {
  data() {
    return {
      count: 0
    }
  },
  methods: {
    increment() {
      this.count++
    }
  },
  mounted() {
    // methods can be called in lifecycle hooks, or other methods!
    this.increment()
  }
}
```
Vue sẽ tự động ràng buộc `this` cho các `method`, điều này giúp cho `method` trỏ đến component để gọi các giá trị. Cho nên tránh sử dụng `arrow function`, điều đó sẽ giúp cho Vue ràng buộc với `this` chuẩn hơn.
```javascript
export default {
  methods: {
    increment: () => {
      // BAD: no `this` access here!
    }
  }
}
```
Các `method` có thể được sử dụng ở template. Trong các template chúng thường được sử dụng như là một hàm bắt sự kiện.
```html
<button @click="increment">{{ count }}</button>
```
### Deep Reactivity
Trong Vue, state sẽ phản ứng với sự thay đổi ngay cả trong một object lồng hay array
```javascript
export default {
  data() {
    return {
      obj: {
        nested: { count: 0 },
        arr: ['foo', 'bar']
      }
    }
  },
  methods: {
    mutateDeeply() {
      // these will work as expected.
      this.obj.nested.count++
      this.obj.arr.push('baz')
    }
  }
}
```
## Computed/watch
### Lý do sử dụng computed
Các biểu thức trong template rất thuận tiện, nhưng chúng không dành cho các nhiệm vụ phức tạp. Nếu đặt quá nhiều logic vào code template, chúng sẽ gây ra trở ngại trong việc kiểm soát và bảo trì.   
Giả sử chúng ta có một object với array trong đó, và ta muốn hiển thị thông báo khác nhau phụ thuộc vào `author` đã có sách hay chưa.
```javascript
export default {
  data() {
    return {
      author: {
        name: 'John Doe',
        books: [
          'Vue 2 - Advanced Guide',
          'Vue 3 - Basic Guide',
          'Vue 4 - The Mystery'
        ]
      }
    }
  }
}
```
```html
<p>Has published books:</p>
<span>{{ author.books.length > 0 ? 'Yes' : 'No' }}</span>
```
Ở ví dụ này, đoạn code ở template đã trở nên lộn xộn hơn, quan trọng hơn, chúng ta không muốn lặp đi lặp lại một biểu thức nhiều lần khi chúng ta cần sử dụng nó trong nhiều template.

Ta có thể cải thiện nó bằng cách sử dụng `computed`:
```javascript
export default {
  data() {
    return {
      author: {
        name: 'John Doe',
        books: [
          'Vue 2 - Advanced Guide',
          'Vue 3 - Basic Guide',
          'Vue 4 - The Mystery'
        ]
      }
    }
  },
  computed: {
    // a computed getter
    publishedBooksMessage() {
      return this.author.books.length > 0 ? 'Yes' : 'No'
    }
  }
}
```
```html
<p>Has published books:</p>
<span>{{ publishedBooksMessage }}</span>
```
Ở đây chúng ta đã khởi tạo hàm `publishedBooksMessage`, nó sẽ trả về kết quả `yes` hoặc `no` khi giá trị của `books` thay đổi.

**Các hàm computed thường được sử dụng trong các trường hợp chỉ cần hiển thị cho người dùng, nó không nên sử dụng để thay đổi state của các giá trị, thay vào đó ta sử dụng `methods`**
### Writable Computed
Mặc định `computed` sẽ chỉ dùng getter. Nhưng trong một vài trường hợp nó cần gán giá trị khác cho `computed`, chúng ta có thể khai báo setter:
```javascript
export default {
  data() {
    return {
      firstName: 'John',
      lastName: 'Doe'
    }
  },
  computed: {
    fullName: {
      get() {
        return this.firstName + ' ' + this.lastName
      },
      set(newValue) {
        // Note: we are using destructuring assignment syntax here.
        [this.firstName, this.lastName] = newValue.split(' ')
      }
    }
  }
}
```
### Watchers
Tương tự như `computed`, `watch` sẽ theo dõi sự thay đổi của state, nhưng `watch` có thể làm một vài hành động "side effect" khi các state thay đổi, ví dụ như thay đổi DOM hoặc thay đổi state dựa vào kết quả của một hàm bất đồng bộ.
```javascript
export default {
  data() {
    return {
      question: '',
      answer: 'Questions usually contain a question mark. ;-)'
    }
  },
  watch: {
    // whenever question changes, this function will run
    question(newQuestion, oldQuestion) {
      if (newQuestion.indexOf('?') > -1) {
        this.getAnswer()
      }
    }
  },
  methods: {
    async getAnswer() {
      this.answer = 'Thinking...'
      try {
        const res = await fetch('https://yesno.wtf/api')
        this.answer = (await res.json()).answer
      } catch (error) {
        this.answer = 'Error! Could not reach the API. ' + error
      }
    }
  }
}
```
```html
<p>
  Ask a yes/no question:
  <input v-model="question" />
</p>
<p>{{ answer }}</p>
```
### Deep Watchers
Mặc định `watch` sẽ không update khi các 'nested-value' thay đổi. Nhưng chúng ta có thể thay đổi việc đó bằng cách cho `watch` thêm thuộc tính `deep : true`. Nhưng lưu ý vì điều này có thể làm giảm hiệu suất của web khi áp dụng với một lượng lớn data.
```javascript
export default {
  watch: {
    someObject: {
      handler(newValue, oldValue) {
        // code here
      },
      deep: true
    }
  }
}
```
### Eager Watchers
Mặc định `watch` sẽ không gọi callback cho đến khi giá trị theo dõi được thay đổi. Nhưng giả sử chúng ta muốn fetch một vài dữ liệu khởi tạo, rồi sau đó fetch lại khi dữ liệu khi các state liên quan thay đổi. Chúng ta có thể sử dụng `immediate : true` để làm việc đó:
```javascript
export default {
  // ...
  watch: {
    question: {
      handler(newQuestion) {
        // this will be run immediately on component creation.
      },
      // force eager callback execution
      immediate: true
    }
  }
}
```
### Callback Flush Timing
Khi thay đổi một state, Vue sẽ kích hoạt thay đổi của component và cả watcher.

Mặc định, watcher được tạo bởi chúng ta sẽ được gọi trước lúc thay đổi component. Điều này có nghĩa là khi bạn muốn truy cập vào DOM ở trong callback của watcher, DOM sẽ là giá trị trước khi state thay đổi.

Nếu chúng ta muốn truy cập vào DOM sau khi nó đã thay đổi, chúng ta có thể sử dụng `flush : post`:
```javascript
export default {
  // ...
  watch: {
    key: {
      handler() {},
      flush: 'post'
    }
  }
} 
```
### `this.$watch()`
Chúng ta có thể tạo một watcher bằng cách dùng phương thức `$watch`:
```javascript
export default {
  created() {
    this.$watch('question', (newQuestion) => {
      // ...
    })
  }
}
```
Điều này sẽ giúp chúng ta trong việc tạo điều kiện khởi tạo một watcher. Nó cũng giúp ta dừng một watcher tùy ý.
### Stopping a Watcher
Một watcher sẽ dừng lại khi component của nó bị xóa khỏi DOM. Nhưng có một vài trường hợp bạn muốn dừng watcher sớm hơn, chúng ta sẽ dùng `$watch()`.
```javascript
const unwatch = this.$watch('foo', callback)
// ...when the watcher is no longer needed:
unwatch()
```
## Template Refs
`ref` cho phép chúng ta lấy tham chiếu từ một phần tử DOM hoặc một component con sau khi nó đã được mount. Ví dụ cho việc sử dụng `ref`, chẳng hạn bạn muốn `focus` vào một `input` sau khi component đã được mount.
```html
<input ref="input">
```
### Truy cập vào các Ref
Kết quả của `ref` được đưa vào `this.$refs`:
```javascript
<script>
export default {
  mounted() {
    this.$refs.input.focus()
  }
}
</script>

<template>
  <input ref="input" />
</template>
```
### Refs trong vòng lặp v-for
> phiên bản  >= 3.2.25
Khi `ref` được sử dụng trong `v-for`, kết quả của `ref` sẽ là mảng chứa các phần tử trong `v-for`.
```javascript
<script>
export default {
  data() {
    return {
      list: [
        /* ... */
      ]
    }
  },
  mounted() {
    console.log(this.$refs.items)
  }
}
</script>

<template>
  <ul>
    <li v-for="item in list" ref="items">
      {{ item }}
    </li>
  </ul>
</template>
```
### Function Refs
Thay vì sử dụng một chuỗi, chúng ta cũng có thể gán cho `ref` một function, function sẽ được chạy khi component được cập nhập.
```html
<input :ref="(el) => { /* assign el to a property or ref */ }">
```
Để có thể gán một function thay vì một chuỗi chúng ta sử dụng `:ref`
### Ref trong một Component
Khi sử dụng `ref` trong một component con, component cha sẽ có quyền truy cập tất cả các thuộc tính trong đó.
```javascript
<script>
import Child from './Child.vue'

export default {
  components: {
    Child
  },
  mounted() {
    // this.$refs.child will hold an instance of <Child />
  }
}
</script>

<template>
  <Child ref="child" />
</template>
```
Chúng ta có thể hạn chế truy cập bằng cú pháp `expose`
```javascript
export default {
  expose: ['publicData', 'publicMethod'],
  data() {
    return {
      publicData: 'foo',
      privateData: 'bar'
    }
  },
  methods: {
    publicMethod() {
      /* ... */
    },
    privateMethod() {
      /* ... */
    }
  }
}
```
Ở ví dụ trên, component cha chỉ có thể truy cập đến `publicData` và `publicMethod` của component con.
## Class & Style binding
### Binding HTML Classes
#### Binding to Objects 
Chúng ta có thể truyền một object vào `:class` để có thể chủ động chuyển đổi class.
```html
<div :class="{ active: isActive }"></div>
```
Ví dụ trên thể hiện class `active` có thể được hiện ra dựa vào logic của `isActive`.

`:class` có thể chứa cả class chủ động lẫn class tĩnh.
```html
<div
  :class="{ active: isActive,
  'text-danger': hasError,
  'static' : true}"
></div>
```
hoặc ta có thể làm theo cách khác.
```html
<div
  class="static"
  :class="{ active: isActive,
  'text-danger': hasError }"
></div>
```
Chúng ta có thể truyền vào một object.
```javascript
data() {
  return {
    classObject: {
      active: true,
      'text-danger': false
    }
  }
}
```
```html
<div :class="classObject"></div>
```
#### Binding to array
Chúng ta có thể truyền vào `:class` một array chứa danh sách các class.
```html
<div :class="[activeClass, errorClass]"></div>
```
Nó sẽ hiển thị tên class dựa trên giá trị logic của chúng.
#### With Components
Khi `class` được sử dụng trên component, chúng sẽ được thêm vào phần tử được bọc trong nó và hợp lại với các class có sẵn của phần tử.
```html
<!-- child component template -->
<p class="foo bar">Hi!</p>
```
```html
<!-- when using the component -->
<MyComponent class="baz boo" />
```
Ở đây kết quả sẽ là:
```html
<p class="foo bar baz boo">Hi</p>
```
Nó cũng tương tự đối với `:class`
### Binding Styles
Cách dùng tương tự class 
## Condition rendering
### `v-if`
`v-if` sẽ dựa vào điều kiện đã cung cấp để hiển thị một khối component và nó chỉ hiển thị khi giá trị đã cho là dạng `truthy`
```html
<h1 v-if="awesome">Vue is awesome!</h1>
```
### `v-else`
Có thể dùng `v-else` để đi chung với một `v-if`, nó được dùng để hiển thị khối component nếu giá trị đưa vào của `v-if` thuộc dạng `falsy`.
```html
<button @click="awesome = !awesome">Toggle</button>

<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>
```
### `v-else-if`
Như cái tên, nó được dùng như một khối 'else if' dành cho `v-if`.
```html
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```
### `v-show`
Có chức năng tương tự như `v-if`, chỉ khác một điều là nó sẽ luôn được render và giữ lại trong DOM bất kể giá trị; `v-show` sẽ thay đổi thuộc tính `display` trong CSS của phần tử đã cho.

`v-show` không thể làm việc với `template` hay với cả `v-else`
## List rendering
### `v-for`
Chúng ta có thể dùng `v-for` để render danh sách các item trong một mảng. Để sử dụng `v-for` : `item in items`, trong đó `items` là mảng dữ liệu chính và `item` là một ánh xạ lên phần tử con ở trong mảng.
```javascript
data() {
  return {
    items: [{ message: 'Foo' }, { message: 'Bar' }]
  }
}
```
```html
<li v-for="item in items">
  {{ item.message }}
</li>
```
`v-for` cũng cung cấp cho chúng ta một ánh xạ thứ hai là `index`, nó sẽ đánh số cho item hiện tại.
```javascript
data() {
  return {
    parentMessage: 'Parent',
    items: [{ message: 'Foo' }, { message: 'Bar' }]
  }
}
```
```html
<li v-for="(item, index) in items">
  {{ parentMessage }} - {{ index }} - {{ item.message }}
</li>
```
```
Parent - 0 - Foo
Parent - 1 - Bar
```
### `v-for` với một object
Chúng ta có thể dùng `v-for` để lặp qua các phần tử trong object. Thứ tự lặp sẽ phụ thuộc vào giá trị khi gọi phương thức `Object.keys()`.
```javascript
data() {
  return {
    myObject: {
      title: 'How to do lists in Vue',
      author: 'Jane Doe',
      publishedAt: '2016-04-10'
    }
  }
}
```
```html
<ul>
  <li v-for="value in myObject">
    {{ value }}
  </li>
</ul>
```
Chúng ta cũng có thể dùng tham số thứ 2 khi lặp một object, đó là tên của thuộc tính, còn được gọi là `key`:
```html
<li v-for="(value, key) in myObject">
  {{ key }}: {{ value }}
</li>
```
Và một cái nữa cho `index`:
```html
<li v-for="(value, key, index) in myObject">
  {{ index }}. {{ key }}: {{ value }}
</li>
```
### `v-for với một khoảng
`v-for` có thể lấy một số nguyên, nó sẽ lặp lại số lần ấy dựa vào khoảng `1..n`.
```html
<span v-for="n in 10">{{ n }}</span>
```
### `v-for` với `v-if`
Hạn chế dùng `v-for` đi cùng với `v-if` vì mức độ ưu tiên của `v-if` cao hơn. Thay vào đó chúng ta có thể dùng `v-for` để bọc `v-if` lại.
```html
<ul>
  <template v-for="user in users" :key="user.id">
    <li v-if="user.isActive">
      {{ user.name }}
    </li>
  </template>
</ul>
```
### Maintaining State with `key`
Để chỉ danh từng phần tử trong `v-for` chúng ta sử dụng `key`.
```html
<div v-for="item in items" :key="item.id">
  <!-- content -->
</div>
```
Điều này giúp cho Vue không nhầm lẫn giữa các phần tử, ngăn tình trạng render sai ngoài ý muốn.
## Event handling
Để lắng nghe một sự kiện, chúng ta sử dụng `v-on`, có thể viết tắt thành ký tự `@`, cú pháp đầy đủ của nó sẽ là `v-on:click='handler'`   
Giá trị của `handler` có thể là:
- **Inline handler**: Inline javascript được thực thi khi sự kiện được kích hoạt. Thường được dùng trong các trường hợp đơn giản.
```javascript
data() {
  return {
    count: 0
  }
}
```
```html
<button @click="count++">Add 1</button>
<p>Count is: {{ count }}</p>
```
- **Method handler**: Một phương thức được định nghĩa ở trong component.
```javascript
data() {
  return {
    name: 'Vue.js'
  }
},
methods: {
  greet(event) {
    alert(`Hello ${this.name}!`)
    // `event` is the native DOM event
    if (event) {
      alert(event.target.tagName)
    }
  }
}
```
```html
<!-- `greet` is the name of the method defined above -->
<button @click="greet">Greet</button>
```
### Truy cập vào tham số `event` trong Inline handler
Khi chúng ta cần truy cập một sự kiện DOM. Ta chỉ cần cho vào phương thức một tham số đặc biệt `$event`, hoặc dùng arrow function:
```html
<!-- using $event special variable -->
<button @click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>

<!-- using inline arrow function -->
<button @click="(event) => warn('Form cannot be submitted yet.', event)">
  Submit
</button>
```
```javascript
methods: {
  warn(message, event) {
    // now we have access to the native event
    if (event) {
      event.preventDefault()
    }
    alert(message)
  }
}
```
### Event Modifiers
Gọi hàm `event.preventDefault()` hay `event.stopPropagation()` là một việc bình thường khi chúng ta xử lý sự kiện. Nhưng thay vì phải gọi chúng mỗi lần khởi tạo hàm xử lý, chúng ta có thể thêm vào `v-on` một bổ ngữ, nó có tiền tố là dấu `.`, ví dụ `.stop`, `.prevent`,....
```html
<!-- the click event's propagation will be stopped -->
<a @click.stop="doThis"></a>

<!-- the submit event will no longer reload the page -->
<form @submit.prevent="onSubmit"></form>

<!-- modifiers can be chained -->
<a @click.stop.prevent="doThat"></a>

<!-- just the modifier -->
<form @submit.prevent></form>

<!-- only trigger handler if event.target is the element itself -->
<!-- i.e. not from a child element -->
<div @click.self="doThat">...</div>
```
### Bắt sự kiện của bàn phím
Cú pháp sẽ là `@keyboardevent.key-kebab`
```html
<input @keyup.page-down="onPageDown" />
```
### `.exact` Modifier
Có thể điều khiển chính xác tổ hợp phím cùng lúc khi cần để kích hoạt sự kiện.
```html
<!-- this will fire even if Alt or Shift is also pressed -->
<button @click.ctrl="onClick">A</button>
<!-- this will only fire when Ctrl and no other keys are pressed -->
<button @click.ctrl.exact="onCtrlClick">A</button>
<!-- this will only fire when no system modifiers are pressed -->
<button @click.exact="onClick">A</button>
```
## Form input binding
Khi cần đồng bộ các state với tham số được nhập vào thẻ `input`, ta sẽ dùng `v-model`, cú pháp:
```html
<input v-model="text">
```
`v-model` có thể dùng với các thẻ input khác nhau, chẳng hạn `textarea`, `select`.
- `input` với kiểu văn bản sẽ dùng thuộc tính `value`
- `input` kiểu `checkbox` hoặc `radio` dùng giá tị `checked` và sự kiện `change`
- `select` dùng `value` như giá trị và `change` như sự kiện
### Ví dụ cơ bản
#### Text
```html
<p>Message is: {{ message }}</p>
<input v-model="message" placeholder="edit me" />
```
#### Checkbox
```javascript
export default {
  data() {
    return {
      checkedNames: []
    }
  }
}
```
```html
<div>Checked names: {{ checkedNames }}</div>

<input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
<label for="jack">Jack</label>

<input type="checkbox" id="john" value="John" v-model="checkedNames">
<label for="john">John</label>

<input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
<label for="mike">Mike</label>
```
Ở đây, mảng `checkedNames` sẽ luôn chứa giá trị của các input đã tích.
### Radio 
```html
<div>Picked: {{ picked }}</div>

<input type="radio" id="one" value="One" v-model="picked" />
<label for="one">One</label>

<input type="radio" id="two" value="Two" v-model="picked" />
<label for="two">Two</label>
```
### Select 
```html
<div>Picked: {{ picked }}</div>

<input type="radio" id="one" value="One" v-model="picked" />
<label for="one">One</label>

<input type="radio" id="two" value="Two" v-model="picked" />
<label for="two">Two</label>
```
### Bind giá trị 
#### Checkbox
```html
<input
  type="checkbox"
  v-model="toggle"
  true-value="yes"
  false-value="no" />
```
Các thuộc tính `true-value` và `false-value` chỉ dùng được khi có `v-model`. Chúng ta cũng có thể gán cho chúng dynamic value bằng `v-bind`
```html
<input
  type="checkbox"
  v-model="toggle"
  :true-value="dynamicTrueValue"
  :false-value="dynamicFalseValue" />
```
#### Radio 
```html
<input type="radio" v-model="pick" :value="first" />
<input type="radio" v-model="pick" :value="second" />
```
`first` sẽ được gán giá trị `pick` khi radio input được chọn và tương tự đối với `second`.
#### Select Options
```html
<select v-model="selected">
  <!-- inline object literal -->
  <option :value="{ number: 123 }">123</option>
</select>
```
`v-model` cũng hỗ trợ bind các giá trị không phải chuỗi. Như ví dụ trên, khi `option` được chọn, `selected` sẽ được gán object `{number : 123}`
#### Modifiers
#### `.lazy`
Thông thường, `v-model` sẽ đồng bộ với những thứ được nhập vào `input`. Bạn có thể thêm `lazy` vào cho nó để nó đồng bộ sau sự kiện `change`.
```html
<!-- synced after "change" instead of "input" -->
<input v-model.lazy="msg" />
```
#### `.number`
Nếu muốn người dùng nhập vào văn bản chỉ là số, chúng ta dùng bổ ngữ `number` sau `v-model`.
```html
<input v-model.number="age" />
```
#### `.trim`
Nếu muốn xóa khoảng cách ở hai bên văn bản.
```html
<input v-model.trim="msg" />
```
## Lifecycle hooks
### Khởi tạo một lifecycle hook
Ví dụ như, `mounted` hook có thể dùng để chạy code sau khi các component đã hoàn thành việc khởi tạo ban đầu và tạo các DOM node.
```javascript
export default {
  mounted() {
    console.log(`the component is now mounted.`)
  }
}
```
Cũng có nhiều hook khác có thể được gọi để thực thi code ở các thời điểm khác nhau của lifecycle, những hook thường được sử dụng là `mounted`, `updated` và `unmounted`.

Tất cả hook đều được gọi với `this` của chúng đang trỏ vào component đã gọi chúng. Cho nên tránh sử dụng arrow function khi khởi tạo một lifecycle hook vì nó sẽ không thể truy cập được đến component bằng `this`.
### Lifecycle Diagram
![](https://vuejs.org/assets/lifecycle.16e4c08e.png)
Có thể xem thêm các [Api hook](https://vuejs.org/api/options-lifecycle.html) khác.
