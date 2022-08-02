# Tổng quan Gitflow
Gitflow là một quy trình làm việc phân nhánh được sử dụng trong các dự án lớn.

Gitflow cơ bản sẽ chia các nhánh làm việc riêng theo mục đích cụ thể ví dụ như :
- Main
- Develop
- Feature
- Hotfix
- Release

 Để khởi tạo một gitflow project ta sử dụng `git flow init`

## Cách chia branch của Gitflow
#### Main branch và Feature branch
Đầu tiên chúng ta có 2 branch chính dó là **Main branch** và **Feature branch**. Main branch chứa bản release chính thức của dự án, còn Develop branch chứa chức năng của phiên bản kế tiếp.
#### Feature branch
Tiếp theo khi muốn tạo một chức năng mới, chúng ta sẽ sử dụng **Feature branch**.Mỗi chức năng chỉ nên có một feature branch và khi muốn làm chung thì có thể pull về. Branch này dùng develop branch như nhánh cha và không tương tác trực tiếp với main branch.
 Cú pháp khởi tạo : `git flow feature start <feature-branch>` 

Khi hoàn thành chức năng trong feature branch chúng ta sẽ merge vào branch develop.

Cú pháp hoàn thành :`git flow feature finish <feature-branch>`
#### Release branch
Khi đã hoàn thành đầy đủ chức năng của một phiên bản trong dự án, chúng ta sẽ sử dụng **Release branch** để fixbug, tạo document, và các định hướng cho bản phát hành kế tiếp, quan trọng nhất đó là không thêm chức năng mới trong branch này. Khi các công việc trên đã hoàn thành thì chúng ta sẽ tiến hành việc merge branch này vào main và tag nó với version number, và nó cũng cần phải merge vào develop.
Cú pháp khởi tạo : `git flow release start <release-version>(e.g: 0.1.0)`
Cú pháp hoàn thành : `git flow release finish '0.1.0'`

#### Hotfix branch
Branch này được dùng như một bản vá khẩn cấp và được trực tiếp chia nhánh ra từ main branch. Khi hoàn thành hotfix có thể merge vào main và develop, lúc merge thì main sẽ được tag phiên bản cập nhập
cú pháp khởi tạo : `git flow hotfix start hotfix_branch`
cú pháp hoàn thành : ` git flow hotfix finish hotfix_branch`

#### Sơ đồ chia nhánh
![example](https://wac-cdn.atlassian.com/dam/jcr:cc0b526e-adb7-4d45-874e-9bcea9898b4a/04%20Hotfix%20branches.svg?cdnVersion=447 "Sơ đồ chia nhánh")

Reference : 
[1] : https://viblo.asia/p/co-ban-ve-gitflow-workflow-4dbZNn6yZYM
[2] : https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow