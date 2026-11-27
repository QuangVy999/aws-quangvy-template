---
title: "Blog 2"
date: 2026-11-27
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Tìm hiểu Amazon Cognito và ứng dụng xác thực người dùng trong dự án Cloud Library Management System

## 1. Tổng quan về Amazon Cognito

Amazon Cognito là dịch vụ của AWS hỗ trợ quản lý danh tính và xác thực người dùng cho các ứng dụng web và di động.

Thay vì phải tự xây dựng toàn bộ hệ thống xác thực từ đầu, nhà phát triển có thể sử dụng Amazon Cognito để triển khai những chức năng như đăng ký tài khoản, đăng nhập, xác minh thông tin liên hệ và khôi phục mật khẩu.

### Một số thành phần cơ bản trong Amazon Cognito

- **User Pools:** Quản lý tài khoản người dùng và thực hiện xác thực.

- **Identity Pools:** Hỗ trợ cấp thông tin xác thực AWS tạm thời để ứng dụng truy cập tài nguyên AWS theo quyền được cấu hình.

Trong phạm vi dự án Cloud Library, em tập trung tìm hiểu và sử dụng Amazon Cognito User Pools.

## 2. User Pool và App Client

Khi cấu hình Amazon Cognito, hai thành phần em cần tìm hiểu đầu tiên là User Pool và App Client.

### User Pool

User Pool là nơi quản lý tài khoản của những người sử dụng ứng dụng.

Tùy theo cấu hình, người dùng có thể đăng ký bằng email, xác minh tài khoản và đăng nhập bằng phương thức xác thực được cho phép.

### App Client

App Client là cấu hình dành cho ứng dụng kết nối với User Pool.

App Client xác định những phương thức xác thực và thiết lập liên quan mà ứng dụng có thể sử dụng.

Trong Cloud Library, em đã tạo User Pool và App Client tại AWS Region Asia Pacific (Singapore), sau đó sử dụng cấu hình này để thử nghiệm chức năng tài khoản của hệ thống.

Qua bước này, em hiểu rõ hơn cách ứng dụng kết nối với dịch vụ xác thực được quản lý trên AWS.

## 3. Ứng dụng Amazon Cognito vào Cloud Library

Cloud Library Management System là hệ thống thư viện trực tuyến với nhiều nhóm người dùng, bao gồm:

- Độc giả (Reader).
- Tác giả (Author).
- Thủ thư (Librarian).
- Quản trị viên (Administrator).

Mỗi nhóm người dùng có những chức năng riêng. Vì vậy, ngoài việc quản lý thông tin tài khoản, hệ thống còn cần có cơ chế xác thực để xác định người đang truy cập.

Trong quá trình phát triển, em đã thực hành và kiểm tra những chức năng cơ bản sau.

### Đăng ký tài khoản

Người dùng nhập các thông tin cần thiết trên giao diện đăng ký của Cloud Library.

Ứng dụng gửi yêu cầu đăng ký đến Amazon Cognito để tạo tài khoản trong User Pool.

### Xác minh email

Sau khi đăng ký, người dùng cần hoàn thành bước xác minh email theo cấu hình của hệ thống.

Em đã thử nghiệm giao diện nhập mã xác nhận và quá trình xác minh tài khoản trước khi đăng nhập.

### Đăng nhập

Sau khi tài khoản được xác nhận, người dùng có thể đăng nhập vào Cloud Library.

Khi thông tin đăng nhập hợp lệ, Amazon Cognito hỗ trợ xác thực và trả về các Token tương ứng với luồng xác thực được cấu hình.

Ứng dụng có thể sử dụng kết quả xác thực để xác định phiên đăng nhập và bảo vệ những chức năng yêu cầu người dùng đăng nhập.

### Kết hợp với dữ liệu ứng dụng

Trong Cloud Library, Amazon Cognito đảm nhận chức năng xác thực, còn Amazon DynamoDB được sử dụng để lưu trữ dữ liệu nghiệp vụ.

Ví dụ, khi người dùng thêm một cuốn sách vào danh sách yêu thích, hệ thống cần liên kết thao tác đó với đúng tài khoản.

Trong quá trình thử nghiệm, em đã kiểm tra việc lưu danh sách yêu thích và xác nhận dữ liệu vẫn tồn tại sau khi đăng xuất rồi đăng nhập lại.

Qua đó, em hiểu rõ hơn vai trò riêng của dịch vụ xác thực và cơ sở dữ liệu trong cùng một ứng dụng.

## 4. Phân biệt xác thực và phân quyền

Một điểm em nhận thấy cần phân biệt rõ khi xây dựng hệ thống có nhiều loại tài khoản là Authentication và Authorization.

### Authentication – Xác thực

Authentication là quá trình xác minh người dùng là ai, chẳng hạn thông qua email và mật khẩu.

### Authorization – Phân quyền

Authorization là quá trình xác định người dùng đã được xác thực có quyền thực hiện những thao tác nào trong hệ thống.

Ví dụ, trong Cloud Library:

| Nhóm người dùng | Chức năng |
|---|---|
| Độc giả (Reader) | Xem sách và lưu sách vào danh sách yêu thích. |
| Tác giả (Author) | Gửi sách để xét duyệt. |
| Thủ thư (Librarian) | Thực hiện các chức năng quản lý nghiệp vụ thư viện. |
| Quản trị viên (Administrator) | Thực hiện các chức năng quản trị hệ thống. |

Amazon Cognito hỗ trợ phần xác thực và cung cấp thông tin danh tính.

Tuy nhiên, các quy tắc phân quyền nghiệp vụ vẫn cần được ứng dụng và Backend kiểm tra phù hợp, không thể chỉ dựa vào việc ẩn hoặc hiển thị chức năng trên giao diện.

Đây cũng là nội dung em cần tiếp tục tìm hiểu khi hệ thống có nhiều nhóm người dùng với quyền truy cập khác nhau.

## 5. Những khó khăn và kinh nghiệm rút ra

Khi mới tiếp cận Amazon Cognito, em cần thời gian để phân biệt các thành phần như User Pool, App Client và những bước trong quy trình xác minh tài khoản.

Bên cạnh đó, việc kết nối giao diện đăng ký, đăng nhập với AWS cũng yêu cầu kiểm tra kỹ các thông số cấu hình và cách ứng dụng xử lý kết quả trả về.

Thông qua quá trình thực hành, em rút ra một số điểm cần chú ý:

- Cần xác định rõ phương thức đăng ký và đăng nhập ngay từ đầu.
- Nên kiểm tra đầy đủ quy trình đăng ký, xác minh email, đăng nhập và đăng xuất.
- Không nên lưu mật khẩu người dùng trong bảng dữ liệu nghiệp vụ.
- Cần phân biệt xác thực tài khoản với kiểm tra quyền truy cập.
- Không đưa thông tin nhạy cảm như mật khẩu, Token hoặc App Client Secret lên mã nguồn công khai.
- Khi xây dựng các chức năng yêu cầu đăng nhập, Backend cần kiểm tra tính hợp lệ của Token và quyền thực hiện thao tác.

Một bài học quan trọng đối với em là các chức năng liên quan đến tài khoản cần được kiểm thử theo cả quy trình, thay vì chỉ kiểm tra riêng việc đăng nhập thành công.

## 6. Kết luận

Qua quá trình tìm hiểu và thực hành với Amazon Cognito, em nhận thấy việc sử dụng một dịch vụ xác thực được quản lý giúp giảm bớt những công việc phải tự xây dựng và vận hành.

Đối với dự án Cloud Library Management System, Amazon Cognito là một thành phần quan trọng trong quá trình phát triển các chức năng tài khoản người dùng.

Bên cạnh việc biết cách cấu hình User Pool và App Client, em cũng hiểu thêm về sự khác biệt giữa xác thực, phân quyền và quản lý dữ liệu nghiệp vụ.

Trong thời gian tới, em muốn tiếp tục tìm hiểu sâu hơn về cơ chế Token, kiểm tra quyền truy cập và cách bảo vệ API khi tích hợp Amazon Cognito vào các ứng dụng thực tế.

Hy vọng bài viết có thể giúp những bạn mới tiếp cận Amazon Cognito có thêm góc nhìn về cách ứng dụng dịch vụ này vào một dự án web.

### Hình ảnh
![overview](/images/3-Blog/Blog2/Blog1.jpg)
![overview](/images/3-Blog/Blog2/Blog2.jpg)
![overview](/images/3-Blog/Blog2/Blog3.jpg)
![overview](/images/3-Blog/Blog2/Blog4.jpg)
### Link bài Blog
https://www.facebook.com/groups/awsstudygroupfcj/permalink/2349109312520694/?rdid=ZYnH7TNOLWm64Cin#
