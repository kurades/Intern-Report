# Vue Router
## ToC
- [Cài đặt Vue Router vào dự án](#cài-đặt-vue-router)
- [Tạo các Route](#tạo-các-route)
- [Lazy load route](#lazy-load)
- [Dynamic route](#dynamic-route-param)
- [Bắt route không tồn tại](#bắt-một-route-không-tồn-tại-từ-thanh-địa-chỉ)
---
## Cài đặt Vue Router
Tải package vue router
```bash
# Vue 2
npm i vue-router@3
# Vue 3
npm i vue-router
```
Áp dụng vào `main.js`
```javascript
// ở đây cú pháp sẽ sử dụng vue 2
import VueRouter from 'vue-router'
import Home from './views/HomePage.vue'

// Khai báo module vue-router
Vue.use(VueRouter)

const routes = [
  {
      path: '/',
      name: 'Home',
      component: Home,
  },
]
const router = new VueRouter({
  mode: 'history',
  routes
})

new Vue({
  router,
  render: h => h(App),
}).$mount('#app')

```
## Tạo các Route
Các route được khởi tạo sẽ cho vào một mảng chung với các thuộc tính bắt buộc `path`, `component`, `name` là thuộc tính tự chọn có thể có hoặc không :
```javascript
{
    path : '/pathname',
    component : ComponentName,
    name : 'Component'
}

```

Thông thường các component này sẽ cho vào một folder riêng  `views` để dễ kiểm soát
## Lazy load
Mặc định, Vue sẽ tải tất cả component có trong router, điều này dẫn đến việc tối ưu kém, tốn dữ liệu của người dùng. Để tránh tình trạng này Vue đã cung cấp khả năng giúp cho ứng dụng chỉ tải tài nguyên về khi người dùng bấm vào một route nhất định.
```javascript
{
    path: '/brazil',
    name : 'Brazil',
    component : ()=> import('./views/BrazilPage.vue'),
},
```
Việc đặt một arrow function chứa dòng import component sẽ giúp cho Vue chỉ thực thi việc tải tài nguyên chỉ khi cần thiết.
## Dynamic route param
Giả sử, chúng ta muốn tạo một trang chi tiết sản phẩm, vấn đề được đặt ra ở đây là tuy có nhiều sản phẩm nhưng chúng lại sử dụng một bố cục giống nhau. Chúng ta không thể copy từng layout một cho mỗi sản phẩm, vì thế dynamic param ra đời cho việc này:
```javascript
// main.js or route.js
const routes = [
    ...routes,
    {
        path: '/detail/:id',
        component : ()=> import('./views/detail.vue'),
    },
]
// e.g : http://abc.xyz/detail/1
```
Để lấy giá trị param chúng ta dùng `this.$route.params.paramName`

Khi chúng ta di chuyển giữa các trang detail, mặc dù đường dẫn đã thay đổi nhưng view vẫn không thay đổi, đó là vì nó vẫn chỉ là một component `detail`, view chỉ thay đổi khi chúng ta di chuyển giữa các component, vì thế chúng ta phải thêm thuộc tính `:key='$route.path'` ở `router-view` để Vue xác định riêng từng item.
## Bắt một route không tồn tại từ thanh địa chỉ
Trong trường hợp này, chúng ta sẽ bắt tất cả các địa chỉ bằng `path : "*"`, đặt nó ở cuối mảng `routes`. Khi các `route` ở trên không match thì route cuối sẽ bắt tất cả path và khi đó chỉ cần thêm một page 404 là xong:
```javascript
// main.js or route.js
const routes = [
    ...routes,
    {
        path: '*',
        component : ()=> import('./views/ErrorPage.vue'),
    },
]
```