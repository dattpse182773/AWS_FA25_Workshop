---
title: "Blog 1"
date: 2025-09-10
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Thông tin dưới đây chỉ dùng để tham khảo. Vui lòng **không sao chép nguyên văn** cho báo cáo của bạn, bao gồm cả phần cảnh báo này.
{{% /notice %}} -->

# Triển khai giải pháp High Performance Computing cho dự báo thời tiết và sản lượng năng lượng tái tạo chính xác

Lĩnh vực khí tượng từ lâu đã dựa vào các mô hình tính toán để dự đoán thời tiết. Sự phát triển của High-Performance Computing (HPC) và machine learning (ML) đã giúp các dự đoán này trở nên chính xác và đáng tin cậy hơn. Bài viết này trình bày các bước triển khai cụm HPC phục vụ dự báo thời tiết trên nền tảng Amazon Web Services (AWS). Nó phân tích cách các giải pháp dựa trên đám mây mang lại tính linh hoạt và khả năng mở rộng, đồng thời mô tả cách HPC và công nghệ đám mây giúp các chuyên gia khí tượng của Iberdrola tính toán dự báo sản lượng năng lượng tái tạo.

Đối với Iberdrola — đơn vị sản xuất **80,3 TWh năng lượng tái tạo trong năm 2024**, dự báo thời tiết chính xác là yếu tố then chốt. Các yêu cầu quy định được áp dụng tại Tây Ban Nha từ năm 2004 đã bắt buộc dự báo sản lượng năng lượng tái tạo, dẫn đến việc Iberdrola Renovables xây dựng MeteoFlow — một công cụ dự báo nội bộ đã được phát triển liên tục suốt 20 năm.

Qua thời gian, MeteoFlow đã phát triển để tích hợp các kỹ thuật mô phỏng khí tượng tiên tiến, ML, AI và công nghệ dữ liệu lớn. Với sự hỗ trợ của đội ngũ chuyên gia đa ngành, hệ thống hiện cung cấp các dự đoán sản lượng năng lượng ngắn hạn và dài hạn chính xác cho nhiều cơ sở năng lượng tái tạo.

MeteoFlow tạo ra dự báo sản lượng cho các trang trại điện gió trên bờ, ngoài khơi, các nhà máy quang điện và các cơ sở thủy điện toàn cầu của Iberdrola — dựa trên dự báo gió, bức xạ mặt trời và các dữ liệu khí tượng khác. Các dự báo này hỗ trợ tham gia thị trường năng lượng, lập kế hoạch O&M, và đánh giá rủi ro.

Các mô hình khí tượng hiện đại giải các phương trình vi phân phi tuyến phức tạp như phương trình Navier–Stokes, đòi hỏi năng lực tính toán quy mô lớn và khả năng truy cập dữ liệu tốc độ cao. Những yêu cầu này khiến điện toán đám mây — với tài nguyên tính toán đàn hồi và dung lượng lưu trữ hầu như không giới hạn — trở thành lựa chọn lý tưởng để vận hành các tác vụ dự báo của MeteoFlow.

---

## Giá trị kinh doanh

MeteoFlow mang lại giá trị kinh doanh quan trọng bằng cách cung cấp dự báo sản lượng điện chính xác với thời gian:
- **96 giờ (dự báo theo giờ)**
- **10 ngày (dự báo theo ngày)**  
cho **450+ nhà máy điện gió và điện mặt trời**.

Dự báo chính xác giúp Iberdrola:
- Tăng doanh thu  
- Giảm mức phạt do sai lệch dự báo thị trường  
- Tăng tính cạnh tranh của năng lượng tái tạo  
- Tối ưu hóa hoạt động O&M  
- Nâng cao quản lý rủi ro  

MeteoFlow còn cung cấp:
- Bản đồ thời tiết qua hệ thống intranet của Iberdrola  
- Cảnh báo rủi ro thời gian thực (gió mạnh, sương giá, bão…)  
- Dự báo dòng chảy cho các nhà máy thủy điện  
- Dự báo sóng cho các tài sản năng lượng ngoài khơi  
- Công cụ hiển thị và phân tích dữ liệu qua *MeteoWeb*  

---

## Kiến trúc MeteoFlow trên AWS

Việc chuyển hệ thống lên AWS tập trung vào ba yêu cầu:

1. **Xử lý hơn 300 TB dữ liệu khí tượng** ở quy mô lớn  
2. **Dự báo thời gian thực**, hỗ trợ ra quyết định kịp thời  
3. **Tính linh hoạt và khả năng mô-đun hóa**, giúp tích hợp tốt hơn và cải thiện cộng tác giữa các nhóm  

Kiến trúc sử dụng các dịch vụ sau:

### **Amazon S3**
Lưu trữ dữ liệu mô hình khí tượng toàn cầu và khu vực (IFS, GFS, ICON-EU, và các mô hình nội bộ tùy chỉnh).

### **Amazon EC2 HPC Instances**
Ví dụ: **hpc7a.96xlarge**:
- 192 lõi vật lý  
- 768 GiB RAM  
- Kết nối mạng EFA tốc độ đến 300 Gbps  
- CPU AMD EPYC thế hệ thứ 4  
- Tối ưu cho các tác vụ HPC có liên kết chặt  

### **Amazon EFS**
Lưu trữ chia sẻ đàn hồi cho kết quả tính toán HPC.

### **Amazon SageMaker**
Cung cấp phân tích tích hợp và phát triển mô hình ML để nâng cao độ chính xác của dự báo.

---

## Di chuyển hơn 300 TB dữ liệu lên AWS

Ban đầu Iberdrola cân nhắc sử dụng AWS Snowball, nhưng phát hiện hạ tầng LAN tại chỗ sẽ trở thành điểm nghẽn, khiến tốc độ không nhanh hơn so với truyền trực tiếp đến AWS Ireland qua đường kết nối tốc độ cao hiện có.

Thay vào đó, họ sử dụng **AWS DataSync**, giúp:
- Tự động hóa việc truyền dữ liệu lớn  
- Mã hóa và kiểm tra tính toàn vẹn dữ liệu  
- Giảm đáng kể chi phí vận hành  

Các bước di chuyển ở mức cao bao gồm:
1. Thiết lập AWS Direct Connect  
2. Tạo VGW & DX Gateway  
3. Cấu hình Private VIF  
4. Định tuyến lưu lượng qua Direct Connect  
5. Triển khai DataSync agent tại on-prem  
6. Tạo nguồn và đích (S3)  
7. Chạy các tác vụ DataSync  

Quá trình này đã di chuyển thành công toàn bộ dữ liệu lịch sử của MeteoFlow lên Amazon S3.

---

## Kết luận

Việc chuyển MeteoFlow lên AWS Cloud đánh dấu một cột mốc quan trọng trong lịch sử phát triển của hệ thống. AWS giúp Iberdrola sử dụng tài nguyên tính toán và lưu trữ đàn hồi, quản lý tập dữ liệu khổng lồ, và tận dụng các dịch vụ nâng cao như Amazon SageMaker. Với nền tảng đám mây hiện đại, Iberdrola có thể tập trung cải thiện các mô hình dự báo dựa trên ML — qua đó tăng cường hiệu suất sản xuất năng lượng tái tạo và hiệu quả tham gia thị trường.

