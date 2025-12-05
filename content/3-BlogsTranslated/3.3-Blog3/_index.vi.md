---
title: "Blog 3"
date: 2025-09-10
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Thông tin bên dưới chỉ mang tính tham khảo. Vui lòng **không sao chép nguyên văn** vào báo cáo của bạn, bao gồm cả thông báo này.
{{% /notice %}} -->

# Cách DTN tăng tốc dự báo thời tiết vận hành bằng NVIDIA Earth-2 trên AWS

Dự báo thời tiết chính xác và kịp thời đang trở nên ngày càng quan trọng đối với các ngành như năng lượng, nông nghiệp, hàng không và vận tải biển. Các mô hình dự báo thời tiết số (NWP) truyền thống dựa trên vật lý đòi hỏi lượng tài nguyên tính toán rất lớn và thời gian xử lý dài, gây khó khăn cho việc mở rộng khi có các hiện tượng thời tiết cực đoan. Để giải quyết vấn đề này, DTN đã hợp tác với NVIDIA và AWS để tích hợp **NVIDIA Earth-2** — nền tảng mô phỏng thời tiết dựa trên AI — vào hệ thống vận hành, giúp tăng tốc độ dự báo, mở rộng linh hoạt và nâng cao độ chính xác.

Blog này mô tả cách DTN xây dựng quy trình dự báo thời tiết cloud-native sử dụng GPU trên AWS với **Earth2Studio**, **AWS Batch**, **các phiên bản EC2 GPU**, và **Step Functions**. Bài viết cũng trình bày cách hệ thống cải thiện độ chính xác theo dõi bão, giảm rủi ro vận hành và hỗ trợ ra quyết định cho khách hàng toàn cầu của DTN.

---

## Tổng quan về NVIDIA Earth-2

**NVIDIA Earth-2** là nền tảng cấp doanh nghiệp dành cho phát triển và vận hành các mô hình thời tiết và khí hậu dựa trên AI. Nền tảng cung cấp:

- Các pipeline tăng tốc cho mô hình AI dựa trên vật lý  
- Các mô hình thời tiết AI đã huấn luyện sẵn như *FourCastNet*  
- Các mô hình siêu phân giải như *CorrDiff*  
- Các workflow mô phỏng và trực quan hóa  
- Công cụ tích hợp dữ liệu thời gian thực từ các nguồn như Registry of Open Data on AWS  

AI inference giúp giảm đáng kể thời gian dự báo—từ hàng giờ xuống chỉ vài giây—cho phép tạo nhanh các dự báo tổ hợp (ensemble) với chi phí thấp hơn.

---

## Pipeline dự báo thời tiết AI của DTN trên AWS

DTN đã triển khai pipeline inference có khả năng mở rộng, được quản lý hoàn toàn trên AWS bằng Earth2Studio. Một quy trình dự báo điển hình gồm các bước:

1. **Chuẩn bị dữ liệu**  
   - AWS Lambda xử lý và chuẩn bị các điều kiện đầu vào  
   - Các tiện ích Python định dạng dữ liệu phù hợp với mô hình  

2. **Suy luận bằng mô hình AI**  
   - AWS Batch triển khai workload của Earth2Studio trên các phiên bản EC2 GPU (G6e, P5, P6)  
   - FourCastNet tạo dự báo tổ hợp toàn cầu ở độ phân giải 25 km chỉ trong vài giây  

3. **Hậu xử lý và tổng hợp**  
   - Container Python tính toán thống kê dự báo tổ hợp  
   - Độ bất định được đo bằng các chỉ số như độ lệch chuẩn  

> Kiến trúc này cho phép DTN mở rộng khả năng dự báo theo nhu cầu—đặc biệt hữu ích trong mùa bão hoặc các sự kiện thời tiết cực đoan.

---

## Kiến trúc Cloud-Native

Hệ thống sản xuất của DTN được xây dựng dựa trên container và mô hình điều phối theo sự kiện.

### Các thành phần AWS chính:

- **Amazon ECR** – lưu trữ container inference tối ưu GPU  
- **AWS Batch** – lập lịch và quản lý workload GPU/CPU  
- **AWS Step Functions** – điều phối pipeline dự báo nhiều giai đoạn  
- **Amazon S3** – data lake lưu đầu vào, đầu ra và mô hình  
- **Amazon SNS** – kích hoạt thông báo cho quy trình downstream  
- **s3fs-mounted S3 volumes** – tăng tốc truy xuất dữ liệu khí tượng lớn  

Để đảm bảo độ tin cậy, hệ thống được tích hợp:

- Cơ chế retry của Step Functions  
- Dead-letter queues để cô lập lỗi  
- Bộ nhớ đệm cho dữ liệu đầu vào nhằm giảm tải download lặp lại  
- Triển khai hạ tầng hoàn chỉnh bằng **AWS CDK**  

---

## Kiểm chứng và hiệu năng

DTN đã kiểm chứng giải pháp bằng dữ liệu của các hiện tượng như bão Milton, Helene và Lee. Các dự báo tổ hợp từ Earth-2 cho thấy độ tương đồng cao với dữ liệu quan sát BestTrack, chứng minh khả năng mô hình trong việc dự báo quỹ đạo bão nhanh và chính xác.

Hệ thống chính thức vận hành từ tháng 6/2025 sau bốn tháng phát triển, nhờ sự phối hợp giữa chuyên gia AWS và NVIDIA để tinh chỉnh pipeline cho độ ổn định sản xuất.

---

## Tác động kinh doanh

Việc tích hợp dự báo thời tiết AI vào hệ thống vận hành giúp DTN cung cấp:

- Dự báo chính xác và kịp thời hơn  
- Khả năng đánh giá rủi ro tốt hơn cho các hiện tượng thời tiết mạnh  
- Cải thiện hiệu suất vận hành của khách hàng  
- Dashboard hỗ trợ ra quyết định mạnh mẽ dựa trên “Decision-Grade Data” của DTN  

Điều này giúp khách hàng trong các lĩnh vực năng lượng, nông nghiệp, logistics và chuỗi cung ứng lên kế hoạch và ứng phó hiệu quả hơn.

---

## Lộ trình phát triển tương lai

DTN dự định mở rộng năng lực dự báo AI bằng cách:

- Mở rộng phạm vi dự báo (từ theo giờ đến cận mùa)  
- Cải thiện các chỉ số đánh giá độ tin cậy dự báo  
- Hỗ trợ thêm nhiều mô hình AI của nền tảng Earth-2  
- Tích hợp sâu hơn với hệ thống phân tích khí hậu nâng cao  

Sự linh hoạt của NVIDIA Earth-2 và các dịch vụ AWS giúp DTN tiếp tục dẫn đầu trong đổi mới dự báo thời tiết bằng AI.
