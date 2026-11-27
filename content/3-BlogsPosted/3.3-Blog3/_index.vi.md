---
title: "Blog 3"
date: 2026-11-27
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Tìm hiểu AWS Amplify: Cách mình nhìn lại kiến trúc và những lưu ý khi dùng thực tế

## 1. AWS Amplify thực chất là gì?

Hiểu đơn giản, AWS Amplify không phải là một hạ tầng mới tách biệt khỏi AWS. Đây là một lớp bọc điều phối nằm phía trên các dịch vụ lõi quen thuộc như Amazon S3, Amazon CloudFront, Amazon Cognito, Amazon DynamoDB và AWS Lambda.

Thay vì phải mở từng dịch vụ để cấu hình thủ công và thiết lập quyền IAM giữa các dịch vụ, Amplify gom những quy trình đó lại thành một nền tảng thống nhất, giúp lập trình viên Frontend và Mobile có thể triển khai sản phẩm nhanh chóng hơn.

### Phân biệt AWS Amplify Gen 1 và Gen 2

Khi tìm hiểu tài liệu của AWS, em nhận thấy AWS Amplify có hai thế hệ là Gen 1 và Gen 2. Điểm khác biệt cốt lõi nằm ở cách thức phát triển và triển khai ứng dụng.

#### Amplify Gen 1

- Chủ yếu thao tác thông qua các câu lệnh hỏi đáp trên Amplify CLI hoặc giao diện Amplify Studio.
- Tự động tạo các Template CloudFormation ở bên dưới để triển khai tài nguyên AWS.

#### Amplify Gen 2

- Chuyển sang hướng phát triển Code-first trên nền tảng AWS CDK.
- Toàn bộ phần Backend như Auth, Data, Storage và Functions được khai báo trực tiếp bằng TypeScript.
- Mỗi thành viên trong nhóm có thể sử dụng một môi trường Cloud Sandbox riêng để kiểm thử mà không ảnh hưởng đến dữ liệu của nhau.

## 2. Kiến trúc hoạt động của AWS Amplify

Nhìn vào sơ đồ kiến trúc, cách AWS Amplify vận hành có thể được chia thành hai luồng chính: luồng triển khai ứng dụng của lập trình viên và luồng truy cập ứng dụng của người dùng.

### Luồng triển khai ứng dụng

1. Lập trình viên viết code trên Visual Studio Code và thực hiện `git push` lên GitHub.
2. GitHub gửi tín hiệu (Webhook) đến AWS Amplify.
3. AWS Amplify tự động lấy mã nguồn mới, cài đặt thư viện bằng lệnh `npm ci`, đóng gói ứng dụng bằng lệnh `npm run build` và triển khai lên môi trường chạy thực tế kèm theo HTTPS.

Một điểm em cảm thấy rất hữu ích là cơ chế **Atomic Deploy**. Nếu bản Build mới xảy ra lỗi trong quá trình triển khai, phiên bản website đang hoạt động trước đó vẫn được giữ nguyên, giúp hạn chế gián đoạn hệ thống.

### Luồng truy cập ứng dụng của người dùng

Khi người dùng truy cập website, hệ thống có hai thành phần chính là Amplify Hosting và Amplify Backend.

#### Amplify Hosting

Amplify Hosting chịu trách nhiệm triển khai và phân phối giao diện website thông qua mạng CDN toàn cầu.

Dịch vụ hỗ trợ các ứng dụng như:

- Single Page Application (SPA): React, Vue.
- Server-Side Rendering (SSR): Next.js, Nuxt.

#### Amplify Backend

Amplify Backend hỗ trợ bốn nhóm tính năng cơ bản của một ứng dụng.

| Thành phần | Chức năng | Dịch vụ AWS |
|---|---|---|
| Authentication | Đăng ký, đăng nhập, phân quyền và cấp JWT Token. | Amazon Cognito |
| Storage | Lưu trữ hình ảnh, file PDF và tài liệu. | Amazon S3 |
| Database | Lưu trữ dữ liệu ứng dụng với khả năng tự mở rộng. | Amazon DynamoDB |
| Function | Thực thi các hàm xử lý logic phía Server theo sự kiện. | AWS Lambda |

## 3. Những điểm dễ hiểu nhầm và lưu ý khi dùng thực tế

Trong quá trình tìm hiểu AWS Amplify, em nhận thấy có một số điểm dễ gây nhầm lẫn đối với những người mới tiếp cận dịch vụ này.

### Sử dụng Amplify Hosting không bắt buộc phải sử dụng Amplify Backend

Amplify Hosting và Amplify Backend có thể được sử dụng độc lập.

Lập trình viên có thể chỉ sử dụng Amplify Hosting để triển khai giao diện ứng dụng, tương tự như Vercel hoặc Netlify.

Trong khi đó, phần Backend có thể kết nối với các dịch vụ AWS đã được tạo sẵn như:

- Amazon Cognito.
- Amazon API Gateway.
- AWS Lambda.
- Amazon DynamoDB.
- Amazon S3.

Ngoài ra, ứng dụng cũng có thể kết nối với Backend được triển khai bên ngoài AWS.

### Lỗi 404 khi tải lại trang trên ứng dụng SPA

Sau khi triển khai ứng dụng SPA như React hoặc Vue, trang chủ có thể hoạt động bình thường.

Tuy nhiên, khi người dùng truy cập một trang con, chẳng hạn như `/books/123`, rồi thực hiện tải lại trang bằng phím F5, hệ thống có thể trả về lỗi `404 Not Found`.

Nguyên nhân là Server không tìm thấy file vật lý tương ứng với đường dẫn được yêu cầu.

Để xử lý vấn đề này, có thể thực hiện các bước sau:

1. Truy cập AWS Amplify Console.
2. Chọn mục **Hosting**.
3. Truy cập **Rewrites and redirects**.
4. Thêm quy tắc `200 (Rewrite)` để chuyển các đường dẫn SPA phù hợp về `/index.html`.

### Cẩn thận với biến môi trường ở Frontend

Trên AWS Amplify Console có mục **Environment variables**, giúp lập trình viên dễ dàng cấu hình các thông tin như đường dẫn API hoặc ID của Amazon Cognito.

Tuy nhiên, những biến môi trường bắt đầu bằng `VITE_` hoặc `NEXT_PUBLIC_` có thể được đóng gói vào mã nguồn phía trình duyệt người dùng.

Vì vậy, tuyệt đối không được lưu trữ những thông tin nhạy cảm như:

- AWS Access Key.
- AWS Secret Access Key.

Đây là một vấn đề bảo mật cần đặc biệt lưu ý trong quá trình phát triển và triển khai ứng dụng.

### Khi nào không nên sử dụng AWS Amplify?

AWS Amplify giúp đơn giản hóa quá trình triển khai ứng dụng. Tuy nhiên, trong một số trường hợp, việc tự cấu hình từng dịch vụ AWS riêng biệt có thể mang lại khả năng kiểm soát và tùy chỉnh tốt hơn.

Một số trường hợp có thể kể đến như:

- Hệ thống yêu cầu tùy chỉnh sâu về mạng nội bộ với kiến trúc VPC phức tạp.
- Hệ thống sử dụng kiến trúc Microservices trên Kubernetes (Amazon EKS).
- Hệ thống yêu cầu tùy chỉnh chi tiết các chính sách Cache trên Amazon CloudFront hoặc các cấu hình AWS WAF.

Trong những trường hợp này, việc tự kết hợp các dịch vụ AWS riêng lẻ sẽ giúp lập trình viên chủ động hơn trong quá trình thiết kế và quản lý hạ tầng.

## 4. Tổng kết

AWS Amplify không thay thế việc tìm hiểu và nắm vững các dịch vụ lõi của AWS, nhưng giúp rút ngắn đáng kể thời gian từ khi viết code đến khi có một sản phẩm hoạt động thực tế trên nền tảng Cloud.

Thông qua việc tìm hiểu luồng hoạt động từ `git push` đến Amplify Hosting và cách kết nối với các thành phần Backend bên dưới, em hiểu rõ hơn về quy trình triển khai ứng dụng trên AWS.

Việc nắm rõ kiến trúc hoạt động của AWS Amplify không chỉ giúp quá trình triển khai ứng dụng trở nên nhanh chóng hơn mà còn hỗ trợ việc mở rộng hoặc tách riêng các thành phần hạ tầng khi dự án phát triển trong tương lai.

### Hình ảnh
![overview](/images/3-Blog/Blog3/Blog1.jpg)
### Link bài Blog
https://www.facebook.com/groups/660548818043427/my_posted_content
