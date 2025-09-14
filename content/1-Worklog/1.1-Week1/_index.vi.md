---
title: "Worklog Tuần 1"
date: 2025-09-14
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->


### Mục tiêu tuần 1:

* Kết nối, làm quen với các thành viên trong First Cloud Journey.
* Hiểu dịch vụ AWS cơ bản, cách dùng console & CLI.
* Hiểu và kết nối EC2, RDS.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Làm quen với các thành viên FCJ <br> - Đọc và lưu ý các nội quy, quy định tại đơn vị thực tập                                                                                             | 08/09/2025   | 08/09/2025      |
| 3   | - Tìm hiểu AWS và các loại dịch vụ <br>&emsp; + Compute <br>&emsp; + Storage <br>&emsp; + Networking <br>&emsp; + Database <br>&emsp;  <br> - Tạo tài khoản AWS <br>&emsp; + Thiết lập MFA <br>&emsp; + Tạo AdminGroup và AdminUser <br>&emsp; + Xác thực tài khoản <br>&emsp; + Khám phá, cấu hình AWS Management Console <br>&emsp;<br>                                             | 09/09/2025   | 09/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tìm hiểu Amazon Virtual Private Cloud (VPC) và AWS Site-to-Site VPN <br>  - **Thực hành:** <br>&emsp; + Hiểu về khái niệm tường lửa VPC và các bước chuẩn bị <br>&emsp;  + Sửa lỗi cùng anh Thịnh phần lab VPC flow logs <br>&emsp; + Triển khai Amazon EC2 Instance <br>&emsp; + Cấu hình Site to Site VPN| 10/09/2025   | 10/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tiếp tục tìm hiểu về EC2: <br> - **Thực hành:** <br>&emsp; + Khởi tạo Window instance <br>&emsp; + Khởi tạo Linux instance <br>&emsp; + Thực hành với Amazon EC2 <br>&emsp; + Triển khai Node.js trên Amazon Linux <br>&emsp; + Ứng dụng Node.js trên Amazon EC2 Windows<br>&emsp; + Tìm hiểu giới hạn sử dụng tài nguyên bằng dịch vụ IAM <br>&emsp;                 | 11/09/2025   | 11/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - Tìm hiểu về Amazon Relational Database Service (Amazon RDS) <br> - **Thực hành:** <br>&emsp; + Tạo EC2 instance <br>&emsp; + Tạo RDS instance <br>&emsp; + Triển khai ứng dụng <br>&emsp; + Backup và restore <br>&emsp;                                                                                      | 12/09/2025   | 12/09/2025      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 1:

* Hiểu AWS là gì và nắm được các nhóm dịch vụ cơ bản: 
  * Compute
  * Storage
  * Networking 
  * Database
  

* Đã tạo và cấu hình AWS Free Tier account thành công.

* Làm quen với AWS Management Console và biết cách tìm, truy cập, sử dụng dịch vụ từ giao diện web.

* Cài đặt và cấu hình AWS CLI trên máy tính bao gồm:
  * Access Key
  * Secret Key
  * Region mặc định
  

* Sử dụng AWS CLI để thực hiện các thao tác cơ bản như:

  * Kiểm tra thông tin tài khoản & cấu hình
  * Lấy danh sách region
  * Xem dịch vụ EC2
  * Tạo và quản lý key pair
  * Kiểm tra thông tin dịch vụ đang chạy
  

* Có khả năng kết nối giữa giao diện web và CLI để quản lý tài nguyên AWS song song.

* Hiểu về những khái niệm cơ bản của EC2:

  * Hiểu cách tạo và quản lý máy chủ ảo trên cloud.
  * Cách kết nối SSH/RDP để làm việc trực tiếp với server.
  * Cài đặt và triển khai ứng dụng web (Node.js) trên EC2.
  * Quản lý bảo mật, firewall (Security Groups), IP tĩnh (Elastic IP).
  * Học cách tối ưu chi phí khi dùng cloud (chọn loại instance phù hợp, bật/tắt đúng lúc).

* Amazon RDS cung cấp các lợi ích chính:

  * Thay thế dễ dàng cho các instance cơ sở dữ liệu truyền thống.
  * Sao lưu tự động và vá lỗi trong khung giờ bảo trì do khách hàng xác định.
  * Mở rộng, sao chép và tính sẵn có chỉ với một nút nhấn.

