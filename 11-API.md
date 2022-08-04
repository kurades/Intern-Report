# API
## ToC
- [RESTful API]()
- [Cách hoạt động của RESTful web API]()
- [Các responstatus cơ bản]()

---
## RESTful API
Là một tiêu chuẩn dùng trong việc thiết kế API cho các ứng dụng web để tiện cho việc quản lý tài nguyên. Nó chú trọng vào tài nguyên hệ thống, bao gồm các trạng thái tài nguyên được định dạng và truyền tải qua HTTP.
![](https://topdev.vn/blog/wp-content/uploads/2019/04/restful-api.jpg)

**API** (Application Programming Interface) là một tập các quy tắc và cơ chế được định nghĩa rằng, một ứng dụng hay một thành phần sẽ tương tác với một ứng dụng hay một thành phần khác. API có thể trả về dữ liệu cần thiết cho ứng dụng của ta ở những kiểu dữ liệu như JSON hay XML.

**REST** (**RE**presentational **S**tate **T**ransfer) là một dạng chuyển đổi cấu trúc dữ liệu, một kiến trúc thiết kế API. Nó sử dụng phương thức HTTP để tạo giao tiếp giữa các máy. Vì vậy, thay vì sử dụng một URL cho việc xử lý một số thông tin của người dùng, REST gửi một yêu cầu HTTP như GET, POST, DELETE và PUT đến một URL để xử lý dữ liệu.

RESTful API là một tiêu chuẩn dùng trong việc thiết kế các API cho các ứng dụng web để quản lý cacs resource. RESTful là một trong những kiểu thiết kế API được sử dụng phổ biến ngày nay để cho các ứng dụng giao tiếp với nhau.

Chức năng quan trong nhất của REST là quy định cách sử dụng các phương thức HTTP(GET, POST, PUT, DELETE) và cách định dạng các URL cho ứng dụng web để quản lý các resource.

## Cách hoạt động của API
![](https://images.viblo.asia/c502a773-8ac5-4f33-bbf8-fa56916b70dc.png)
REST hoạt động chủ yếu dựa vào giao thức HTTP. Các hoạt động cơ bản nêu trên sẽ sử dụng những phươngthức HTTP riêng.
- POST(**C**reate) : Tạo một tài nguyên.
- GET(**R**ead) : Trả về tài nguyên hay một danh sách các tài nguyên.
- PUT(**U**pdate) : Cập nhập tài nguyên.
- DELETE(**D**elete) : Xóa một tài nguyên.

Các hoạt động này được gọi tắt là CRUD.

Và thông thường, RESTful API không sử dụng cookie và session mà sử dụng một access_token cho mỗi lần request.

## Các status code phổ biến
Khi gửi một request, chúng ta sẽ có một vài status code để nhận biết tình trạng của request:
- 200 OK - request thành công và dựa vào phương thức HTTP sẽ có một loại định nghĩa.
- 201 Created - request thành công, và một tài nguyên đã được tạo, thông thường sẽ nhận được khi dùng POST hoặc PUT.
- 204 No content - Trả về khi tài nguyên đã xóa thành công.
- 304 Not Modified - Client có thể sử dụng dữ liệu cache.
- 400 Bad Request - Request không hợp lệ.
- 401 Unauthorize - Chưa đăng nhập.
- 403 Forbidden - Bị từ chối.
- 404 Not Found - Không tìm thấy tài nguyên từ URI.
- 405 Method Not Allowed - Phương thức không cho phép với user.
- 429 Too Many Request - Request bị từ chối do giới hạn request.

Có thể xác định cơ bản được kiểu response bằng số đầu tiên của status:
- (100-199) - Thông tin
- (200-299) - Thành công
- (300-399) - Điều hướng
- (400-499) - Lỗi Client
- (500-599) - Lỗi Server