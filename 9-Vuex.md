# Cơ bản về Vuex
## ToC
- [Cơ bản về Vuex](#cơ-bản-về-vuex)
  - [ToC](#toc)
  - [Vuex là gì](#vuex-là-gì)
  - [Cài đặt](#cài-đặt)
  - [Concept cơ bản](#concept-cơ-bản)
    - [States](#states)
      - [Cú pháp `mapState`](#cú-pháp-mapstate)
    - [Getters](#getters)
    - [Mutations](#mutations)
      - [Các điểm cần chú ý khi dùng mutation](#các-điểm-cần-chú-ý-khi-dùng-mutation)
    - [Actions](#actions)
      - [Dispatching Actions](#dispatching-actions)
      - [Dispatch actions trong component](#dispatch-actions-trong-component)
    - [Modules](#modules)
      - [Module Local State](#module-local-state)
      - [Namespace](#namespace)
      - [Truy cập vào các giá trị toàn cục trong một namespaced module](#truy-cập-vào-các-giá-trị-toàn-cục-trong-một-namespaced-module)
      - [Khai báo `Action` toàn cục trong một namespaced module](#khai-báo-action-toàn-cục-trong-một-namespaced-module)
    - [Khung của một dự án Vuex cơ bản](#khung-của-một-dự-án-vuex-cơ-bản)
---
## Vuex là gì
Vuex là một pattern và là thư viện quản lý state dành cho Vue. Nó được xem như là một nơi lưu trữ tập trung dành cho các component trong ứng dụng.

Khi bạn muốn truyền một state qua nhiều component, sẽ xảy ra vài vấn đề sau đây:
- Nhiều view sẽ phụ thuộc vào giá trị của một state
- Hành động trong các view khác nhau có thể cần thay đổi trong một state.

Để xử lý việc này, vuex đã ra đời. Nó như một vùng chứa giúp cho các component trong một project có thể lấy state hoặc method được định nghĩa trong nó và được lấy ra thuận tiện.
## Cài đặt
```sh
# dành cho vue 2
npm install vuex@3 --save
```
Khi dùng với hệ thống module, chúng ta cần phải khai báo như sau :
```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```
Để chứa một state cơ bản :
```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```
Bây giờ bạn có thể truy cập vào object state bằng `store.state` và kích hoạt thay đổi state bằng `store.commit`
```javascript
store.commit('increment')

console.log(store.state.count) // -> 1
```
Để có thể truy cập vào `this.$store` ở một vue component, chúng ta cần phải cung cấp `store` vào Vue root component:
```javascript
new Vue({
  el: '#app',
  store: store,
})
```
## Concept cơ bản
### States
Cách đơn giản nhất để lấy state trong component đó là lấy state từ `computed`
```javascript
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return store.state.count
    }
  }
}
```
Mỗi khi `store.state.count` thay đổi, nó sẽ khiến thuộc tính `computed` kiểm tra lại giá trị và kích hoạt thay đổi DOM.

Tuy nhiên, nếu chúng ta dùng hệ thống module, nó sẽ cần phải import lại `store` trong tất cả component dùng `store state`.

Vuex đã cung cấp một cách để  truyền `store` đến mọi cấp con của component từ component root bằng cách dùng tùy chọn `store`:
```javascript
const app = new Vue({
  el: '#app',
  // provide the store using the "store" option.
  // this will inject the store instance to all child components.
  store,
  components: { Counter },
  template: `
    <div class="app">
      <counter></counter>
    </div>
  `
})
```
Bằng cách truyền vào lựa chọn `store` từ component `root`, `store` sẽ được cung cấp đến tất cả component con của component `root` và sẽ được gọi bằng `this.$store`:
```javascript
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }
  }
}
```
#### Cú pháp `mapState`
Khi một component cần dùng nhiều `store state` cùng một lúc, khởi tạo tất cả thành phần computed sẽ rất dài dòng. Để xử lý việc đó, chúng ta có thể dùng `mapState`, nó có thể tạo một hàm computed getter cho chúng ta:
```javascript
import { mapState } from 'vuex'

export default {
  // ...
  computed: mapState({

    count: state => state.count,

    countAlias: 'count',

    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
}
```
chúng ta có thể truyền vào một mảng chuỗi vào `mapState` :
```javascript
computed: {
    ...mapState([
        // map this.count to store.state.count
        'count'
    ])
}
```
### Getters
Đôi khi chúng ta cần tính toán chuyển hóa state dựa vào `store state`, chẳng hạn lọc một danh sách các phần tử và đếm chúng:
```javascript
computed: {
  doneTodosCount () {
    return this.$store.state.todos.filter(todo => todo.done).length
  }
}
```
Nếu có nhiều hơn một component dùng nó, chúng ta cần phải chép lại hàm, điều đó không cần thiết khi đã có `getter`. Nó như một thuộc tính `computed` của `store`. Khi `state` của nó thay đổi, `getter` sẽ đánh giá lại và thay đổi giá trị. `Getters` sẽ lấy `state` như tham số đầu tiên :
```javascript
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```
Chúng ta có thể sử dụng `mapGetters` để truy cập các giá trị getter:
```javascript
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
    // mix the getters into computed with object spread operator
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}
```
### Mutations
Cách duy nhất để thay đổi state của store đó là sử dụng `mutations`:
```javascript
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      // mutate state
      state.count++
    }
  }
})
```
Bạn không thể gọi trực tiếp một hàm xử lý thay đổi mà phải dùng `store.commit()`:
```javascript
store.commit('increment')
```
Chúng ta có thể truyền một tham số  thêm bằng cách dùng payload:
```javascript
mutations: {
  increment (state, n) {
    state.count += n
  }
}
```
Trong nhiều trường hợp, payload nên là một object vì thế khi chứa nhiều trường, và sự thay đổi cũng sẽ được mô tả rõ hơn.
```javascript
// ...
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
```
```javascript
store.commit('increment', {
  amount: 10
})
```
#### Các điểm cần chú ý khi dùng mutation
- Có thể truyền một object khi sử dụng commit:
```javascript
store.commit({
  type: 'increment',
  amount: 10
})
```
- Dùng hằng số cho kiểu mutation:
```javascript
// mutation-types.js
export const SOME_MUTATION = 'SOME_MUTATION'
```
```javascript
// store.js
import Vuex from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = new Vuex.Store({
  state: { ... },
  mutations: {
    [SOME_MUTATION] (state) {
      // mutate state
    }
  }
})
```
- Mutation nên là hàm đồng bộ:
```javascript
mutations: {
  someMutation (state) {
    api.callAsyncMethod(() => {
      state.count++
    })
  }
}
```
- Commit mutation vào component:
```javascript
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations([
      'increment', // map `this.increment()` to `this.$store.commit('increment')`

      // `mapMutations` also supports payloads:
      'incrementBy' // map `this.incrementBy(amount)` to `this.$store.commit('incrementBy', amount)`
    ]),
    ...mapMutations({
      add: 'increment' // map `this.add()` to `this.$store.commit('increment')`
    })
  }
}
```
### Actions
Tương tự như `mutations` nhưng sự khác nhau đó là :
- Thay vì thay đổi một state thì `actions` lại commit `mutations`.
- `Actions` có thể  chứa một hoạt động bất đồng bộ.
Gán một hành động đơn giản:
```javascript
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }
})
```
Action nhận một object `context` chứa tập hợp các phương thức và thuộc tính của store, vì thế chúng ta có thể gọi `context.commit` hay `context.state` hoặc có thể là `context.getters`.

Chúng ta có thể dùng destruct để tối ưu code:
```javascript
actions: {
  increment ({ commit }) {
    commit('increment')
  }
}
```
#### Dispatching Actions
Các hành động được kích hoạt bằng `store.dispatch`:
```javascript
store.dispatch('increment')
```
Lúc đầu chúng ta sẽ thấy thật buồn cười khi chúng ta có thể dùng mutation để gọi hàm tăng `increment`. Nhưng mutation lại cần đồng bộ, Action thì không, Chúng ta có thể tạo một hành động bất đồng bộ trong một action:
```javascript
actions: {
  incrementAsync ({ commit }) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }
}
```
#### Dispatch actions trong component
Có thể dispatch một hành động trong component bằng `this.$store.dispatch('xxx')` hoặc dùng `mapAction`:
```javascript
import { mapActions } from 'vuex'

export default {
  // ...
  methods: {
    ...mapActions([
      'increment', // map `this.increment()` to `this.$store.dispatch('increment')`

      // `mapActions` also supports payloads:
      'incrementBy' // map `this.incrementBy(amount)` to `this.$store.dispatch('incrementBy', amount)`
    ]),
    ...mapActions({
      add: 'increment' // map `this.add()` to `this.$store.dispatch('increment')`
    })
  }
}
```
### Modules
Khi ứng dụng của chúng ta trở nên lớn hơn, các state và action cũng trở nên lớn hơn và khó kiểm soát. Để ngăn tình trạng đó, Vuex cho phép chúng ta chia store thành nhiều **module**. Mỗi module có thể chứa state, mutation, actions và getters riêng, và chúng cũng có thể chứa nhiều module nhỏ hơn:
```javascript
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> `moduleA`'s state
store.state.b // -> `moduleB`'s state
```
#### Module Local State
Trong một mutation và getter của module, tham số đầu tiên nhận vào là local state của module.
```javascript
const moduleA = {
  state: () => ({
    count: 0
  }),
  mutations: {
    increment (state) {
      // `state` is the local module state
      state.count++
    }
  },

  getters: {
    doubleCount (state) {
      return state.count * 2
    }
  }
}
```
#### Namespace
Mặc định, các `actions` và `mutations` sẽ được gán dưới dạng biến toàn cục, nó cho phép nhiều module có thể phản ứng với loại action/mutation, `Getters` cũng vậy. Nhưng bạn cần phải cẩn thận với việc đặt tên cho 2 getter giống nhau ở một non-namespaced module bởi vì nó có thể dẫn đến lỗi.

Để giúp cho module có thể tách biệt hoặc tái sử dụng, chúng ta có thể sử dụng `namespaced : true`.
```javascript
const store = createStore({
  modules: {
    account: {
      namespaced: true,

      // module assets
      state: () => ({ ... }), // module state is already nested and not affected by namespace option
      getters: {
        isAdmin () { ... } // -> getters['account/isAdmin']
      },
      actions: {
        login () { ... } // -> dispatch('account/login')
      },
      mutations: {
        login () { ... } // -> commit('account/login')
      },

      // nested modules
      modules: {
        // inherits the namespace from parent module
        myPage: {
          state: () => ({ ... }),
          getters: {
            profile () { ... } // -> getters['account/profile']
          }
        },

        // further nest the namespace
        posts: {
          namespaced: true,

          state: () => ({ ... }),
          getters: {
            popular () { ... } // -> getters['account/posts/popular']
          }
        }
      }
    }
  }
})
```
#### Truy cập vào các giá trị toàn cục trong một namespaced module
Nếu muốn sử dụng state và getter toàn cục, `rootState` và `rootGetters` được truyền vào như tham số thứ 3 và thứ 4 trong hàm getter, và cũng có mặt trong object `context` ở hàm `action`.

Để có thể sử dụng các biến toàn cục đó, ta dùng `{root : true}` như tham số thứ 3 của `dispatch` và `commit`.
```javascript
modules: {
  foo: {
    namespaced: true,

    getters: {
      // `getters` is localized to this module's getters
      // you can use rootGetters via 4th argument of getters
      someGetter (state, getters, rootState, rootGetters) {
        getters.someOtherGetter // -> 'foo/someOtherGetter'
        rootGetters.someOtherGetter // -> 'someOtherGetter'
        rootGetters['bar/someOtherGetter'] // -> 'bar/someOtherGetter'
      },
      someOtherGetter: state => { ... }
    },

    actions: {
      // dispatch and commit are also localized for this module
      // they will accept `root` option for the root dispatch/commit
      someAction ({ dispatch, commit, getters, rootGetters }) {
        getters.someGetter // -> 'foo/someGetter'
        rootGetters.someGetter // -> 'someGetter'
        rootGetters['bar/someGetter'] // -> 'bar/someGetter'

        dispatch('someOtherAction') // -> 'foo/someOtherAction'
        dispatch('someOtherAction', null, { root: true }) // -> 'someOtherAction'

        commit('someMutation') // -> 'foo/someMutation'
        commit('someMutation', null, { root: true }) // -> 'someMutation'
      },
      someOtherAction (ctx, payload) { ... }
    }
  }
}
```
#### Khai báo `Action` toàn cục trong một namespaced module
Nếu muốn khai báo `action` toàn cục trong namespaced module, ta có thể đánh dấu nó với `{root:true}` và định nghĩa hàm xử lý trong hàm `handler`:
```javascript
{
  actions: {
    someOtherAction ({dispatch}) {
      dispatch('someAction')
    }
  },
  modules: {
    foo: {
      namespaced: true,

      actions: {
        someAction: {
          root: true,
          handler (namespacedContext, payload) { ... } // -> 'someAction'
        }
      }
    }
  }
}
```
### Khung của một dự án Vuex cơ bản
```sh
├── index.html
├── main.js
├── api
│   └── ... # abstractions for making API requests
├── components
│   ├── App.vue
│   └── ...
└── store
    ├── index.js          # where we assemble modules and export the store
    ├── actions.js        # root actions
    ├── mutations.js      # root mutations
    └── modules
        ├── cart.js       # cart module
        └── products.js   # products module
```