# Cấu hình môi trường làm việc cho dev
### Mục tiêu
- Cài đặt Ubuntu
- cài đặt nodejs sử dụng nvm
- cài đặt vueCLI
- cài đặt vscode
- khởi tạo app vue 

### 1. Cách cài đặt Ubuntu
1. Đầu tiên cần phải có cho mình 1 USB chứa file **.ISO** của hệ điều hành ubuntu ([file tải](https://ubuntu.com/download/desktop)).
2. Chúng ta dùng [Rufus](https://rufus.ie/en/) để tạo bootable USB ![](https://rufus.ie/pics/rufus_en.png)
Để setting theo ảnh ngoại trừ phần **target system** chúng ta sẽ để **_UEFI_** và bấm <kbd>START</kbd>.
3. Tiếp theo chúng ta cung cấp ổ đĩa cho ubuntu
![](https://phongvu.vn/cong-nghe/wp-content/uploads/2018/08/4-1.jpg)
Tùy chỉnh dung lượng muốn cài đặt, tốt nhất trên 35GB. Lý do sẽ cung cấp ở bước tạo phân vùng
 ![](https://phongvu.vn/cong-nghe/wp-content/uploads/2018/08/5-1.jpg)
 Đây là phân vùng đã tạo để chứa ubuntu 
 ![step3](https://phongvu.vn/cong-nghe/wp-content/uploads/2018/08/6-1.jpg)
1. Tiếp theo tắt fast startup ![](https://phongvu.vn/cong-nghe/wp-content/uploads/2018/08/9-1.jpg)
2. Bây giờ cắm USB và khởi động lại máy nếu như không hiện cài đặt ở phía dưới chúng ta có thể bấm <kbd>f2</kbd> hoặc <kbd>f12</kbd> sau đó chọn tên của USB ![](https://phongvu.vn/cong-nghe/wp-content/uploads/2018/08/10-1.jpg)
3. Chọn tiếp install ubuntu ![](https://phongvu.vn/cong-nghe/wp-content/uploads/2018/08/11-1.jpg)
Bỏ qua một vài bước thiết lập, đến phần **QUAN TRỌNG** nhất
7. Đến phần này chọn *Something else* ![](https://phongvu.vn/cong-nghe/wp-content/uploads/2018/08/15.jpg)
8. Bấm vào phân vùng đã chia ở **Bước 3**, chúng ta sẽ chia phần vùng này thành 3 phần *root*, *swap* và *home* ![](https://phongvu.vn/cong-nghe/wp-content/uploads/2018/08/16.jpg)
9. Tạo phân vùng swap như ảnh dưới và lưu ý kích thước của phân vùng này sẽ thay đổi tùy thuộc vào RAM của máy(vd: máy có RAM 8GB thì swap partition là 8192MB). Tham khảo bảng dưới:

| **Amount of RAM installed in system** | **Recommended swap space** |
| --- | --- |
| $\le$ 2GB | X2 RAM |
| 2GB - 8GB | = RAM |
| $>$ 8GB | 8GB |

 ![](https://phongvu.vn/cong-nghe/wp-content/uploads/2018/08/18.jpg)
10.  Tạo phân vùng root và home như ảnh dưới. Ở phần mount point, *root* sẽ là `/` còn *home* sẽ là `/home`, và size của 2 vùng này tối thiểu là 6GB mỗi partition, tốt nhất là chia 10 - 15GB cho mỗi phân vùng tùy vào nhu cầu cá nhân![](https://phongvu.vn/cong-nghe/wp-content/uploads/2018/08/17.jpg)
11. Cuối cùng bấm <kbd>Install now</kbd> để hoàn thành thiết lập. Các bước còn lại là thiết lập cơ bản nên sẽ bỏ qua.

### 2. Cài đặt linh tinh lặt vặt
1. nodejs sử dụng nvm
Để cài đặt nvm chúng ta cần tải **Install script** và chạy nó bằng lệnh:
    ```sh
    wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
    ```
    Sau đó cài đặt environment để có thể chạy lệnh nvm trên terminal 
    ```sh
    export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
    ```
    Để cài đặt node chúng ta dùng lệnh : 
    ```sh
    nvm install node # cài đặt phiên bản mới nhất
    # hoặc
    nvm install 14.7.0 # cài đặt phiên bản cụ thể
    ```
    Để chạy chạy một node version cụ thể chúng ta dùng lệnh : 
    ```sh
    nvm use node # chạy phiên bản mới nhất
    # hoặc 
    nvm use 14 # chạy phiên bản node 14
    ```
    Để thiết lập một phiên bản node làm mặc định chúng ta dùng :
    ```sh
    nvm alias default 14 # thiết lập phiên bản node 14 làm mặc định
    ```
2. Cài đặt vueCLI và tạo một Vue 2 project
   Để cài đặt vueCLI chúng ta dùng : 
   ```sh
    npm install -g @vue/cli
   ```
   Để tạo một Vue project
   ```sh
    vue create vue-project-name
   ```
   sau đó nó sẽ hiện 2 phương án, tùy vào yêu cầu dự án chúng ta sẽ chọn dùng Vue 2 hoặc Vue 3.
3. Cài đặt vscode
   Đầu tiên chúng ta tải file .deb ở [trang chủ](https://code.visualstudio.com/Download)
   Sau đó dùng lệnh: 
   ```sh
    sudo apt install đường_dẫn/đến/file.deb
   ```


**Reference:** 

https://phongvu.vn/cong-nghe/huong-dan-dual-boot-ubuntu-windows-10/

https://opensource.com/article/19/2/swap-space-poll

https://ubuntu.com/tutorials/install-ubuntu-desktop

https://github.com/nvm-sh/nvm