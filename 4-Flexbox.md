# Flexbox 101
### 1. Tại sao lại cần Flexbox
Flexbox được dùng để cung cấp khả năng trình bày, căn lề và chia khoảng cách giữa các mục cho dù bất kể kích thước không biết được hoặc được kéo dãn tùy ý.

Flexbox có thể cho thẻ chứa khả năng thay đổi **width** và **height** của các item bên trong để có thể lấp khoảng trống của thẻ chứa hoặc làm các item co lại để ngăn chúng tràn ra ngoài thẻ chứa.

Flexbox đa phần được dùng trong các thành phần có quy mô bố cục nhỏ, và chỉ cần căn theo một chiều, còn **Grid** lại sử dụng cho quy mô lớn hơn và sử dụng để căn bố cục 2 chiều.

### 2. Các đặc tính quan trọng trong Flexbox

Property    | value(s)  | Use in    | Description
-------     | -------   | -----     | -------
display     | flex      | container | Xác định một flex container
flex-direction | row, row-reverse, column, column-reverse | container | Xác định main-axis của container và hướng đặt của các item trong nó
flex-wrap   | nowrap, wrap, wrap-reverse | container | Mặc định flex sẽ cố căn các item trong một hàng(**nowrap**), khi dùng **wrap** nó sẽ căn các item đến các hàng tiếp theo, dùng **wrap-reverse** nó sẽ bọc item trong nhiều dòng nhưng theo chiều ngược lại
flex-flow | `<direction> <wrap>` | container | kết hợp giữa flex-direction và flex-wrap
justify-content | flex-start(end), center, space-between(evenly,around) | container | Căn theo main-axis
align-items | flex-start(end), center, stretch, baseline | container | Căn các item theo cross-axis, **stretch** sẽ làm cho các item giãn ra lấp đầy container
align-content | flex-start(end), center, stretch, space-between(around) | container | *Chỉ áp dụng khi container có nhiều dòng*<br> Công dụng tương đương align-items
gap, row-gap, column-gap | 10px, 10px 20px(row column) | container | Khoảng cách giữa các item
order | `integer` | item | Mặc định là 0 khi sửa giá trị này, thứ tự của item sẽ thay đổi theo thứ tự tăng dần
flex-grow | `integer` | item | Cho item khả năng dãn ra nếu cần thiết. Nếu các item có giá trị grow bằng nhau, chúng sẽ giãn ra bằng nhau. Nếu chỉ có một item có giá trị lớn, nó sẽ cố giãn ra lớn gấp giá trị đó so với các item
flex-shrink | `integer` | item | Cho item khả năng co lại nếu cần thiết.
flex-basis | 200px | item | Xác định kích thước mặc định của item, mặc định của đặc tính này là auto
flex | `<flex-grow> <flex-shrink> <flex-basis>` | item | Kết hợp giữa flex-grow, flex-shrink và flex-basis
align-self | flex-start(end), center, baseline, stretch | item | Cho một item nhất định khả năng căn theo cross-axis

### Exercise
**Sử dụng flexbox để xây dừng layout cho module chat messager.**

![](https://few-mustard-099.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F548bb0d1-3492-49e7-9f73-03df90108248%2FUntitled.png?table=block&id=231d8b6a-7a51-464c-bdde-06dda9a743fa&spaceId=ec107eff-8d61-4d83-a22e-5ab345aee518&width=800&userId=&cache=v2)

[Source](https://github.com/kurades/flexbox)

[Demo](https://kurades.github.io/flexbox/)

**Reference :**

[Hubspot](https://blog.hubspot.com/website/css-grid-vs-flexbox#:~:text=CSS%20Grid%20and%20Flexbox%20are,to%20create%20one%2Ddimensional%20layouts.)

[css-tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)