# Vuetify
## Toc
- [Vuetify](#vuetify)
  - [Toc](#toc)
  - [Vuetify là gì](#vuetify-là-gì)
  - [Cài đặt Vuetify vào dự án Vue](#cài-đặt-vuetify-vào-dự-án-vue)
  - [Chức năng](#chức-năng)
    - [Xem 2 chiều](#xem-2-chiều)
    - [Display Breakpoints](#display-breakpoints)
    - [Icon fonts](#icon-fonts)
    - [Theme](#theme)
  - [Style và animation](#style-và-animation)
    - [Color](#color)
    - [Flex](#flex)
    - [Spacing](#spacing)
    - [Văn bản và kiểu chữ](#văn-bản-và-kiểu-chữ)
    - [Transition](#transition)
  - [Component của Vuetify](#component-của-vuetify)
    - [Application](#application)
    - [Button](#button)
    - [Cards](#cards)
  - [Điều hướng](#điều-hướng)
  - [Custom Vuetify](#custom-vuetify)

---
## Vuetify là gì
Vuetify là một UI Framework dành cho Vue. Không như những framework khác, vuetify thiết kế theo kiểu 'ground up' cho nên rất dễ để tiếp cận và nó dựa trên các component của [Material Design](https://material.io/).

Vuetify dựa vào hướng tiếp cận 'mobile first' để thiết kế, nghĩa là ứng dụng của bạn sẽ luôn chuẩn xác dù sử dụng ở trên điện thoại, máy tính bảng hay máy tính.
## Cài đặt Vuetify vào dự án Vue
Nếu đang sử dụng Vue-cli, đơn giản chỉ cần sử dụng:
```sh
vue add vuetify
```
Nó sẽ sửa đổi một số file cơ bản nên để tránh trường hợp mất mát dữ liệu, nên commit code trước.

## Chức năng 
### Xem 2 chiều
Vuetify có hỗ trợ các ngôn ngữ RTL(right to left) và chỉ cần kích hoạt bằng lựa chọn **rtl**:
```javascript
import Vue from 'vue'
import Vuetify from 'vuetify/lib'

Vue.use(Vuetify)

export default new Vuetify({
  rtl: true,
})
```
Chúng ta có thể sử dụng icon được custom:
```javascript
import Vue from 'vue'
import Vuetify from 'vuetify/lib'
import myIconSvg from 'myIcon.svg'

Vue.use(Vuetify)

export default new Vuetify({
  icons: {
    iconfont: 'fa',
    values: {
      customIconSvg: myIconSvg,
      customIconSvgPath: 'M14.989,9.491L6.071,0.537C5.78,0.246,5.308,0.244,5.017,0.535c-0.294,0.29-0.294,0.763-0.003,1.054l8.394,8.428L5.014,18.41c-0.291,0.291-0.291,0.763,0,1.054c0.146,0.146,0.335,0.218,0.527,0.218c0.19,0,0.382-0.073,0.527-0.218l8.918-8.919C15.277,10.254,15.277,9.784,14.989,9.491z',
    },
  },
})
```
### Display Breakpoints
Vuetify có thể điều khiển các phần tử dựa trên kích thước của window. Tính năng này dựa trên **Grid** và một vài **responsive class**
| Screen size         | Margin  | Body    | Layout columns |
|---------------------|---------|---------|----------------|
| Extra-small (phone) |         |         |                |
| 0-599dp             | 16dp    | Scaling | 4              |
| | | |
| Small (tablet)      |         |         |                |
| 600-904             | 32dp    | Scaling | 8              |
| 905-1239            | Scaling | 840dp   | 12             |
| | | |
| Medium (laptop)     |         |         |                |
| 1240-1439           | 200dp   | Scaling | 12             |
| | | |
| Large (desktop)     |         |         |                |
| 1440+               | Scaling | 1040    | 12             |

Nhiều hơn về [Break point](https://vuetifyjs.com/en/features/breakpoints/)
### Icon fonts
Vuetify được khởi động với các Icon của Material Design, Material Icon và Font Awesome, mặc định sẽ dùng Material Design.

Để sử dụng hoặc thay đổi thư viện icon, chúng ta sử dụng tùy chọn `iconfont`.
```javascript
import Vue from 'vue'
import Vuetify from 'vuetify/lib'

Vue.use(Vuetify)

export default new Vuetify({
  icons: {
    iconfont: 'mdiSvg', // 'mdi' || 'mdiSvg' || 'md' || 'fa' || 'fa4' || 'faSvg'
  },
})
```
### Theme
- Vuetify có hỗ trợ cả theme sáng tối.
- Có thể custom màu sắc theme theo ý thích
```javascript
import Vue from 'vue'
import Vuetify from 'vuetify/lib'

const vuetify = new Vuetify({
  theme: {
    themes: {
      light: {
        primary: '#3f51b5',
        secondary: '#b0bec5',
        accent: '#8c9eff',
        error: '#b71c1c',
      },
    },
  },
})
```
## Style và animation
### Color 
Có thể khai báo màu trong một background bằng `<div class="red">` hay `<span class="red--text">` cho chữ. Chúng đều có hỗ trợ **darken** và **lighten** bằng cách sử dụng `text--{lighten|darken}-{n}`

Vuetify hỗ trợ một bảng màu cho sẵn, có thể tham khảo ở [đây](https://vuetifyjs.com/en/styles/colors/#material-colors)
### Flex
Để sử dụng flexbox chúng ta truyền `d-flex` vào class cần dùng.

Sử dụng các thuộc tính khác của flex ở [đây](https://vuetifyjs.com/en/styles/flex/).
### Spacing
Để sử dụng, chúng ta dùng theo cú pháp `{property}{direction}-{size}`. Size sẽ nằm trong khoảng `0-16`

Những **property** có thể áp dụng vào là:
- `m` cho `margin`
- `p` cho `padding`

**Direction** có thể áp dụng vào :
- `t` cho `top`
- `b` cho `bottom`
- `l` cho `left`
- `r` cho `right`
- `x` cho trục ngang
- `y` cho trục dọc
- `a` cho tất cả hướng

Có thể [xem thêm](https://vuetifyjs.com/en/styles/spacing/#negative-margin)
### Văn bản và kiểu chữ
Vuetify cung cấp khả năng thay đổi kiểu chữ với các breakpoint khác nhau. Nó được dùng theo cú pháp sau : `{property}-{breakpoint}-{value}`
Giá trị của `property` có thể là:
- `h1` -> `h6`
- `subtitle-1`, `subtitle-2`
- `body-1`, `body-2`
- `button`
- `caption`
- `overline`

Các API khác dành cho văn bản và kiểu chữ có thể xem ở [đây](https://vuetifyjs.com/en/styles/text-and-typography/)
### Transition
Vuetify cung cấp cho chúng ta nhiều hoạt ảnh động đa dạng, có thể xem các API của chúng ở [đây](https://vuetifyjs.com/en/styles/transitions).

Ta có thể tự tạo một transition của riêng mình:
```javascript
import { createSimpleTransition } from 'vuetify/lib/components/transitions/createTransition'

const myTransition = createSimpleTransition('my-transition')

Vue.component('my-transition', myTransition)
```
Hàm `createSimpleTransition` chấp nhận một tham số đó là **tên**. Nó sẽ dùng tên đó để hook với style của bạn. Đây là ví dụ của `v-fade-transition`:
```css
.fade-transition
  &-leave-active
    position: absolute

  &-enter-active, &-leave, &-leave-to
    transition: $primary-transition

  &-enter, &-leave-to
    opacity: 0
```
## Component của Vuetify
Vuetify cho chúng ta nhiều component với các chức năng khác nhau, bạn có thể xem ở [đây](https://vuetifyjs.com/en/components/alerts/).

Bây giờ chúng ta chỉ nói về những component quan trọng trong một ứng dụng.
### Application
Trong vuetify, component `v-app` và prop `app` trong component như `v-navigation-drawer`,`v-app-bar`, `v-footer`,... giúp chúng ta tự khởi động ứng dụng với kích thước phù hợp xung quanh component `v-main`. Component `v-app` là **CẦN THIẾT** đối với mọi ứng dụng, đây là mỏ neo cho rất nhiều component của vuetify. `v-app` nên chỉ được render **một lần duy nhất**.

Bạn có thể đặt layout element ở bất cứ đâu, miễn là bạn sử dụng thuộc tính `app`. Component chính để làm cho trang của bạn làm việc ổn định đó là `v-main`. Component `v-main` sẽ chủ động chỉnh sửa kích thước phụ thuộc vào sườn của ứng dụng đã được chỉ định.

Khi sử dụng `vue-router`, tốt nhất nên đặt nó ở trong `v-main`.
```html
<!-- App.vue -->
<v-app>
  <v-navigation-drawer app>
    <!-- -->
  </v-navigation-drawer>
  <v-app-bar app>
    <!-- -->
  </v-app-bar>
  <!-- Sizes your content based upon application components -->
  <v-main>
    <!-- Provides the application the proper gutter -->
    <v-container fluid>
      <!-- If using vue-router -->
      <router-view></router-view>
    </v-container>
  </v-main>
  <v-footer app>
    <!-- -->
  </v-footer>
</v-app>
```

Bên dưới là những component hỗ trợ prop `app` và được dùng như một thuộc tính layout ở ứng dụng của bạn.
- `v-app-bar`: Luôn được đặt ở trên cùng của ứng dụng và có độ ưu tiên thấp hơn `v-system-bar`
- `v-bottom-navigation`: Luôn được đặt ở dưới của ứng dụng và có độ ưu tiên cao hơn `v-footer`
- `v-footer`: Luôn được đặt ở dưới ứng dụng và có độ ưu tiên thấp hơn `v-bottom-navigation`
- `v-navigation-drawer`: Có thể được đặt bên trái hoặc phải ứng dụng và có thể config để nằm bên cạnh hay bên dưới `v-app-bar`.
- `v-system-bar`: Luôn được đặt phía trên của ứng dụng và có độ ưu tiên cao hơn `v-app-bar`

### Button
Thay thế button thông thường của html với nhiều chức năng đa dạng và nhiều theme. Cách sử dụng:
- v-btn
- v-btn-toggle
Các chức năng hiện có :
- Block : Giúp cho button mở rộng chiều dài tối đa
- Depressed : Không có box shadow
- Floating : Thường dùng để chứa icon và có hình tròn
- Loading : Bạn có thể thông báo với người dùng rằng tiến trình đang được thực thi.
- Oulined : Áp dụng viền ngoài của button với màu chính của nó.
- Rounded : Tương tự như nút thông thường nhưng nó sẽ bo viền các cạnh.

### Cards
Component `v-card` là một component linh hoạt và có thể dùng cho bất cứ việc gì, từ một bảng đến một hình ảnh tĩnh. Component **card** có nhiều công cụ hỗ trợ giúp cho việc đánh dấu dễ dàng hơn.

**Card** có 4 component chính, `v-card-title`, `v-card-subtitle`, `v-card-text` và `v-card-actions`.

Các props của `v-card`
- Loading : card có thể đặt state loading khi xử lý hành động của người dùng. 
- Oulined : Không có bóng và có một biên nhỏ.

Các chức năng khác của **Card**:
- Card Reveal : Sử dụng `v-expand-transition` và sự kiện `@click`, bạn đã có một thẻ có thể hiện thông tin mỗi khi nút được bấm, kích hoạt thẻ ẩn hiện lên
- Content wrapping : Tiện cho việc bọc các content với nhau.
- Grid : Sử dụng grid để có một bố cục đẹp.
- Horizontal cards : Sử dụng `v-col`, bạn có thể tạo một thẻ ngang tùy chỉnh.
- Media with text : Sử dụng hệ thống bố cục, chúng ta có thể thêm văn bản tùy chỉnh ở bất cứ đâu trên ảnh nền
## Điều hướng
- Click outside : `v-click-outside` gọi một hàm khi một thứ gì đó nằm bên ngoài thuộc tính được chọn được bấm. hàm này mặc định được dùng trong các component `v-menu` hay `v-dialog`
- Intersection observer : `v-intersect` sử dụng để phát hiện xem khi thuộc tính nằm trong khung nhìn của người dùng. Nó cũng được dùng cho component `v-lazy`.
- Mutation observer : `v-mutate` sử dụng khi muốn phát hiện một thuộc tính được cập nhập
- Resize directive : Chúng ta sử dụng `v-resize` lúc cần gọi một hàm khi cửa sổ thay đổi kích thước.
- Scroll directive : `v-scroll` cun cấp cho chúng ta khả năng gọi một callback khi cửa sổ hay một mục tiêu cụ thể (dùng `.self`) cuộn con lăn
## Custom Vuetify
Để custom một component của Vuetify thông thường chúng ta sẽ thay đổi thuộc tính sass của chúng. giả sử chúng ta muốn custom một button mặc định, ta sẽ tìm trên trang web chính của [Vuetify](https://vuetifyjs.com/) API của `v-btn`, ở đó ta sẽ thấy mục **SASS variable** và thay đổi chúng từ đó.
```html
<v-btn color='red' />
```

```html
<style lang="sass">
  @import '~vuetify/src/styles/styles.sass'

  $red : (
    'base' : ##ff0000
  );
</style>
```