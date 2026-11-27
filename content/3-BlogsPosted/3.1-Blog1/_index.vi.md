
---
title: "Blog 1"
date: 2026-11-27
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Tìm hiểu Amazon S3 và ứng dụng lưu trữ tài liệu, hình ảnh sách trong Cloud Library Management System

## 1. Tổng quan về Amazon S3

Amazon S3 (Amazon Simple Storage Service) là dịch vụ lưu trữ đối tượng (Object Storage) do AWS cung cấp. Dịch vụ này cho phép lưu trữ và truy xuất nhiều loại dữ liệu như hình ảnh, tài liệu PDF, video, file sao lưu và những nội dung khác.

Khác với cơ sở dữ liệu dùng để quản lý thông tin có cấu trúc, Amazon S3 tập trung vào việc lưu trữ và quản lý các đối tượng dữ liệu.

### Một số khái niệm cơ bản trong Amazon S3

- **Bucket:** Là nơi chứa các Object. Khi sử dụng Amazon S3, chúng ta cần tạo Bucket trước khi tải file lên.

- **Object:** Là dữ liệu được lưu trong Bucket, chẳng hạn như một file PDF hoặc ảnh bìa sách. Mỗi Object bao gồm nội dung dữ liệu và các thông tin liên quan.

- **Object Key:** Là tên định danh của một Object trong Bucket.

Ví dụ, trong hệ thống thư viện, có thể tổ chức file theo những Object Key như:

    books/covers/book001.jpg
    books/pdfs/book001.pdf
    authors/submissions/book002.pdf

Đây là ví dụ về cách tổ chức dữ liệu, không phải cấu trúc đã được xác nhận trong dự án.

- **Storage Class:** Amazon S3 cung cấp nhiều lớp lưu trữ khác nhau để đáp ứng nhu cầu truy cập và tối ưu chi phí. Việc lựa chọn Storage Class cần dựa trên tần suất sử dụng và yêu cầu truy xuất dữ liệu.

## 2. Nhu cầu lưu trữ file trong Cloud Library

Cloud Library Management System là dự án nhóm xây dựng hệ thống quản lý thư viện và hỗ trợ người dùng đọc sách trực tuyến.

Trong hệ thống, dữ liệu có thể được chia thành hai nhóm chính.

### Dữ liệu thông tin sách

Bao gồm:

- Mã sách.
- Tên sách.
- Tác giả.
- Thể loại.
- Mô tả.
- Những thông tin nghiệp vụ khác.

Đây là những dữ liệu có thể được quản lý bằng Amazon DynamoDB.

### Dữ liệu dạng file

Bao gồm:

- Ảnh bìa sách.
- Tài liệu PDF.
- Những file được tải lên trong quá trình sử dụng hệ thống.

Đối với nhóm dữ liệu thứ hai, Amazon S3 là một giải pháp lưu trữ đáng tìm hiểu vì được thiết kế để quản lý các Object thay vì các bản ghi dữ liệu nghiệp vụ.

Việc tách thông tin sách và nội dung file cũng giúp phân chia trách nhiệm giữa các thành phần của hệ thống rõ ràng hơn.

## 3. Kết hợp Amazon S3 và Amazon DynamoDB

Một hướng thiết kế phù hợp với Cloud Library là sử dụng Amazon DynamoDB để lưu thông tin nghiệp vụ và Amazon S3 để lưu các file liên quan đến sách.

Ví dụ, khi một cuốn sách được thêm vào hệ thống:

1. File ảnh bìa hoặc PDF được tải lên vùng lưu trữ phù hợp.
2. Hệ thống ghi nhận Object Key hoặc thông tin tham chiếu đến file.
3. Thông tin sách được lưu trong DynamoDB.
4. Khi người dùng mở trang chi tiết sách, ứng dụng truy xuất thông tin sách và sử dụng tham chiếu file để hiển thị nội dung phù hợp.

Theo hướng này, DynamoDB không cần trực tiếp lưu toàn bộ nội dung nhị phân của file PDF hoặc ảnh bìa.

Trong quá trình thực hiện Cloud Library, em đã có cơ hội kiểm tra dữ liệu sách được lưu trong bảng DynamoDB và quan sát cách giao diện hiển thị danh mục sách.

Từ đó, em tìm hiểu thêm vai trò của một dịch vụ lưu trữ đối tượng như Amazon S3 đối với những thành phần dữ liệu dạng file.

Tuy nhiên, cách lưu trữ file thực tế vẫn cần được đối chiếu với cấu hình và mã nguồn của dự án.

## 4. Quản lý file PDF và ảnh bìa sách

Đối với một ứng dụng thư viện trực tuyến, ảnh bìa và tài liệu đọc có những yêu cầu truy cập khác nhau.

### Ảnh bìa sách

Ảnh bìa thường được sử dụng khi hiển thị danh mục hoặc trang chi tiết sách.

Nếu lưu trên Amazon S3, ứng dụng có thể tham chiếu đến Object tương ứng để tải và hiển thị ảnh.

Đối với những nội dung được phép truy cập công khai, có thể nghiên cứu thêm Amazon CloudFront để phân phối nội dung hiệu quả mà vẫn kiểm soát cách truy cập Amazon S3.

### File PDF

Khác với ảnh bìa, một số tài liệu PDF có thể yêu cầu người dùng đăng nhập hoặc đáp ứng điều kiện truy cập trước khi đọc.

Vì vậy, khi nghiên cứu lưu trữ tài liệu trên Amazon S3, em cần quan tâm đến quyền truy cập thay vì mặc định cho phép tất cả người dùng tải file trực tiếp.

### File do tác giả gửi lên

Cloud Library có chức năng hỗ trợ tác giả gửi sách để xét duyệt.

Đối với những file đang chờ xét duyệt, cần có cách quản lý riêng để tránh công khai tài liệu trước khi được chấp thuận.

Một hướng thiết kế là lưu trữ những tài liệu này trong vùng dữ liệu riêng và chỉ cấp quyền truy cập cho các thành phần được phép xử lý.

## 5. Bảo mật dữ liệu trên Amazon S3

Trong quá trình tìm hiểu Amazon S3, em nhận thấy việc quản lý quyền truy cập là một nội dung đặc biệt quan trọng.

### Block Public Access

Amazon S3 cung cấp cơ chế Block Public Access nhằm hạn chế việc cấp quyền truy cập công khai ngoài ý muốn.

Đối với hệ thống thư viện, đặc biệt là những tài liệu không được phép truy cập tự do, việc duy trì cấu hình bảo vệ phù hợp là rất cần thiết.

### IAM Policies và Bucket Policies

Những chính sách này giúp kiểm soát danh tính hoặc thành phần nào được phép thực hiện các thao tác như tải lên, đọc hoặc xóa Object.

### Presigned URL

Presigned URL là một cách cấp quyền truy cập có thời hạn đến Object cụ thể mà không cần công khai toàn bộ Bucket.

Ví dụ, sau khi hệ thống kiểm tra rằng người dùng có quyền đọc một tài liệu, Backend có thể tạo Presigned URL để người dùng truy cập file trong khoảng thời gian nhất định.

Tuy nhiên, Presigned URL cần được bảo vệ vì bất kỳ ai sở hữu URL còn hiệu lực đều có thể sử dụng quyền truy cập mà URL đó cấp.

### Mã hóa dữ liệu

Amazon S3 tự động mã hóa các Object mới khi lưu trữ. Ngoài cơ chế mã hóa mặc định, AWS cũng hỗ trợ những lựa chọn khác phù hợp với yêu cầu bảo mật của từng hệ thống.

Qua những nội dung này, em nhận thấy rằng khi sử dụng Amazon S3, việc tải file lên thành công chỉ là bước đầu. Nhà phát triển còn cần quan tâm đến cách người dùng truy cập file và phạm vi quyền được cấp.

## 6. Những lưu ý về chi phí và tổ chức dữ liệu

Bên cạnh bảo mật, chi phí cũng là yếu tố cần cân nhắc khi sử dụng Amazon S3.

Chi phí có thể phụ thuộc vào dung lượng lưu trữ, số lượng yêu cầu, loại lưu trữ và hoạt động truyền dữ liệu.

Đối với Cloud Library, số lượng ảnh bìa và tài liệu PDF có thể tăng khi thư viện bổ sung sách mới. Vì vậy, cần có kế hoạch tổ chức và quản lý dữ liệu hợp lý.

Một số điểm em rút ra khi tìm hiểu Amazon S3 gồm:

- Sử dụng Object Key có quy tắc đặt tên rõ ràng.
- Phân biệt file công khai và file yêu cầu xác thực.
- Kiểm tra loại file và kích thước file khi người dùng tải lên.
- Không cấp quyền truy cập công khai cho toàn bộ Bucket chỉ để hiển thị một số tài liệu.
- Lựa chọn Storage Class phù hợp với tần suất truy cập.
- Theo dõi dung lượng lưu trữ và những yêu cầu phát sinh chi phí.
- Xem xét chính sách vòng đời dữ liệu khi hệ thống có nhiều file không còn được sử dụng.

## 7. Kết luận

Amazon S3 là một dịch vụ lưu trữ đối tượng có thể đáp ứng nhiều nhu cầu quản lý file trong ứng dụng web, đặc biệt đối với những hệ thống có hình ảnh và tài liệu như Cloud Library Management System.

Thông qua việc tìm hiểu Amazon S3, em hiểu rõ hơn sự khác biệt giữa lưu trữ dữ liệu nghiệp vụ trong Amazon DynamoDB và lưu trữ các file dưới dạng Object.

Bên cạnh đó, em cũng nhận thấy rằng thiết kế lưu trữ cần được xem xét cùng với quyền truy cập, bảo mật dữ liệu và chi phí sử dụng.

Trong thời gian tới, em muốn tiếp tục tìm hiểu cách tích hợp Amazon S3 vào quy trình tải lên tài liệu, hiển thị ảnh bìa và cấp quyền đọc file PDF trong hệ thống thư viện.

Hy vọng bài viết có thể mang lại một góc nhìn hữu ích cho những bạn đang tìm hiểu Amazon S3 và cách kết hợp dịch vụ lưu trữ đối tượng với các thành phần khác trong ứng dụng thực tế.

### Hình ảnh
![overview](/images/3-Blog/Blog1/Blog1.jpg)
![overview](/images/3-Blog/Blog1/Blog2.jpg)
### Link bài Blog
https://www.facebook.com/groups/awsstudygroupfcj/permalink/2350500839048208/?rdid=g55e6bkJsxOjuhG5#