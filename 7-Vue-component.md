# Vue component cơ bản 
## ToC
- [Khởi tạo component](#khởi-tạo-một-component)
- [Dùng component](#sử-dụng-một-component)
- [Truyền Props](#truyền-props)
- [Lắng nghe sự kiện](#lắng-nghe-sự-kiện)
- [Phân bổ nội dung bằng slot](#phân-bổ-nội-dung-với-slot)
- [Dynamic Components](#dynamic-component)
--- 
## Khởi tạo một component
Để khởi tạo component, chúng ta sẽ định ngĩa mỗi Vue component trong các file chuyên dụng bằng cách sử dụng đuôi `.vue` 
```HTML
<script>
export default {
  data() {
    return {
      count: 0
    }
  }
}
</script>

<template>
  <button @click="count++">You clicked me {{ count }} times.</button>
</template>
```
## Sử dụng một component
Để sử dụng một component con, chúng ta cần phải `import` nó trong component cha. Giả sử chúng ta có một component `ButtonCounter.vue`, component sau đó sẽ có thể được gọi khi export ra ngoài.
```html
<script>
import ButtonCounter from './ButtonCounter.vue'

export default {
  components: {
    ButtonCounter
  }
}
</script>

<template>
  <h1>Here is a child component!</h1>
  <ButtonCounter />
</template>
```
Để có thể gọi một component đã được import vào template của chúng ta, ta cần phải khai báo nó với thuộc tính `component`. Component này sau đó có thể được gọi như một thẻ. 
### Truyền Props
Để truyền dữ liệu từ component cha sang component con, chúng ta sử dụng `props`.
```html
<!-- BlogPost.vue -->
<script>
export default {
  props: ['title']
}
</script>

<template>
  <h4>{{ title }}</h4>
</template>
```
Khi giá trị được truyền xuống, nó sẽ trở thành giá trị của component đó và có thể truy cập bằng `this`.
Khi chúng ta đã khởi tạo `props`, ta có thể truyền dữ liệu cho nó bằng thuộc tính riêng:
```html
<BlogPost title="My journey with Vue" />
<BlogPost title="Blogging with Vue" />
<BlogPost title="Why Vue is so fun" />
```
Trong một vài trường hợp, chúng ta muốn truyền một mảng của bài đăng :
```javascript
export default {
  // ...
  data() {
    return {
      posts: [
        { id: 1, title: 'My journey with Vue' },
        { id: 2, title: 'Blogging with Vue' },
        { id: 3, title: 'Why Vue is so fun' }
      ]
    }
  }
}
```
Và rồi khi muốn nó render cho từng component, sử dụng `v-for`:
```html
<BlogPost
  v-for="post in posts"
  :key="post.id"
  :title="post.title"
 />
```
Chúng ta có thể cho prop kiểu dữ liệu riêng :
```javascript
export default {
  props: {
    title: String,
    likes: Number
  }
}
```
### Lắng nghe sự kiện
Khi chúng ta phát triển component `<BlogPost>`, có một vài chức năng cần khả năng giao tiếp trở lại với component cha. Ví dụ, chúng ta muốn một tính năng trợ năng để phóng to font chữ của của `<BlogPost>`, khi giữ cho toàn bộ trang font chữ mặc định.

Ở component cha, chúng ta có thể hỗ trợ chức năng này bằng cách thêm đặc tính `postFontSize`
```javascript
data() {
  return {
    posts: [
      /* ... */
    ],
    postFontSize: 1
  }
}
```
Nó sẽ điều khiển kích cỡ chữ của toàn bộ bài blog
```html
<div :style="{ fontSize: postFontSize + 'em' }">
  <BlogPost
    v-for="post in posts"
    :key="post.id"
    :title="post.title"
   />
</div>
```
Bây giờ thêm một nút vào `<BlogPost>`
```html
<!-- BlogPost.vue, omitting <script> -->
<template>
  <div class="blog-post">
    <h4>{{ title }}</h4>
    <button>Enlarge text</button>
  </div>
</template>
```
Bây giờ, nút vẫn chưa làm gì cả, chúng ta muốn bấm vào nút để nói với component cha rằng kích cỡ chữ nên to ra cho mọi chữ trong tất cả post. Để giải quyết việc đó chúng ta sử dụng `v-on`
```html
<BlogPost
  ...
  @enlarge-text="postFontSize += 0.1"
 />
```
Khi đó component con có thể phát ra sự kiện bằng cách gọi hàm `$emit` truyền vào tên của sự kiện đã cung cấp:
```html
<!-- BlogPost.vue, omitting <script> -->
<template>
  <div class="blog-post">
    <h4>{{ title }}</h4>
    <button @click="$emit('enlarge-text')">Enlarge text</button>
  </div>
</template>
```
Chúng ta có thể khai báo sự kiện phá ra bằng `emits`:
```html
<!-- BlogPost.vue -->
<script>
export default {
  props: ['title'],
  emits: ['enlarge-text']
}
</script>
```
## Phân bổ nội dung với slot
Component cha:
```html
<AlertBox>
  Something bad happened.
</AlertBox>
```
Component con:
```html
<template>
  <div class="alert-box">
    <strong>This is an Error for Demo Purposes</strong>
    <slot />
  </div>
</template>
```
Slot như một placeholder giúp cho các nội dung được truyền vào ở đúng vị trí của nó.
### Slot được đặt tên
Đôi lúc chúng ta cần truyền vào nhiều nội dung cùng lúc, khi đó thuộc tính `name` sẽ được sử dụng. Nó sẽ đánh tên cho từng slot và khi cần được gọi, chúng ta sử dụng `v-slot:name`:
```html
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```
```html
<BaseLayout>
  <template v-slot:header>
    <!-- content for the header slot -->
  </template>
</BaseLayout>
```
## Dynamic Component
Đôi lúc chúng ta muốn chủ động thay đổi giữa các component, như là tab.

Điều này có thể thực hiện được bởi `<component>` với thuộc tính đặc biệt là `:is`
```html
<component :is="currentTab"></component>
```
Ở ví dụ trên, giá trị truyền vào `:is` có thể là:
- Tên của component dưới dạng chuỗi
- Object của component