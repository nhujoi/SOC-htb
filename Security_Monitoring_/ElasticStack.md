# Giới thiệu về Elastic Stack

## 1. Elastic Stack là gì?
Elastic Stack (trước đây thường gọi là ELK Stack) là một bộ sưu tập mã nguồn mở gồm ba ứng dụng chính: **Elasticsearch, Logstash và Kibana**. Chúng hoạt động hài hòa để cung cấp khả năng tìm kiếm và trực quan hóa toàn diện cho việc phân tích thời gian thực và khám phá các nguồn tập tin nhật ký (log).

Kiến trúc cấp cao của Elastic Stack có thể được nâng cấp trong các môi trường tiêu tốn tài nguyên bằng cách thêm **Kafka, RabbitMQ, và Redis** để làm bộ đệm và tăng tính đàn hồi, cũng như **nginx** để bảo mật.



## 2. Các thành phần cốt lõi

### Elasticsearch
* Là một công cụ tìm kiếm phân tán dựa trên JSON, được thiết kế với các API RESTful.
* Đóng vai trò là thành phần cốt lõi, xử lý việc đánh chỉ mục (indexing), lưu trữ và truy vấn dữ liệu.
* Cho phép người dùng thực hiện các truy vấn phức tạp và các hoạt động phân tích trên các bản ghi log đã được Logstash xử lý.

### Logstash
* Chịu trách nhiệm thu thập, chuyển đổi và vận chuyển các bản ghi log.
* Điểm mạnh nằm ở khả năng hợp nhất dữ liệu từ nhiều nguồn và chuẩn hóa chúng thông qua ba khu vực hoạt động chính:
    1.  **Xử lý đầu vào (Input):** Nhận log từ các vị trí từ xa (file, TCP socket, syslog) và chuyển đổi sang định dạng máy có thể hiểu.
    2.  **Chuyển đổi và làm giàu (Transform & Enrich):** Sử dụng các plugin bộ lọc (filter plugins) để sửa đổi định dạng hoặc nội dung log dựa trên điều kiện xác định trước.
    3.  **Gửi bản ghi (Output):** Sử dụng plugin đầu ra để truyền bản ghi log đến Elasticsearch.

### Kibana
* Công cụ trực quan hóa cho các tài liệu Elasticsearch.
* Cho phép người dùng xem dữ liệu, thực thi truy vấn và đơn giản hóa việc hiểu kết quả thông qua bảng, biểu đồ và bảng điều khiển (dashboards) tùy chỉnh.



### Beats (Thành phần bổ sung)
* Là các bộ chuyển tiếp dữ liệu (data shippers) nhẹ, đơn mục đích, được cài đặt trên các máy từ xa.
* Chuyển tiếp log và số liệu trực tiếp đến Logstash hoặc Elasticsearch, đơn giản hóa quy trình thu thập dữ liệu.

**Luồng dữ liệu điển hình:**
`Beats -> Logstash -> Elasticsearch -> Kibana` hoặc `Beats -> Elasticsearch -> Kibana`.



---

## 3. Elastic Stack như một giải pháp SIEM
Elastic Stack có thể được sử dụng như một giải pháp Quản lý Sự kiện và Thông tin Bảo mật (SIEM) để thu thập, lưu trữ, phân tích và trực quan hóa dữ liệu bảo mật.

* **Thu thập:** Dữ liệu từ tường lửa, IDS/IPS và các thiết bị đầu cuối (endpoints) được nạp vào qua Logstash.
* **Lưu trữ & Phát hiện:** Elasticsearch lưu trữ, đánh chỉ mục và thực hiện tìm kiếm/tương quan để phát hiện sự cố.
* **Vận hành:** Các nhà phân tích SOC (Security Operations Center) sử dụng Kibana làm giao diện chính để tạo bảng điều khiển tùy chỉnh và hiểu rõ các sự kiện bảo mật.

---

## 4. Ngôn ngữ truy vấn Kibana (KQL)
KQL là ngôn ngữ truy vấn mạnh mẽ, thân thiện với người dùng, được thiết kế để tìm kiếm và phân tích dữ liệu trong Kibana, trực quan hơn so với Query DSL của Elasticsearch.

### Cấu trúc cơ bản và Toán tử
* **Cấu trúc cặp `trường:giá trị`:**
    * Ví dụ: `event.code:4625` lọc các sự kiện có mã sự kiện Windows 4625 (đăng nhập thất bại).
    * Giúp các nhà phân tích SOC xác định các cuộc tấn công brute force hoặc đoán mật khẩu.
* **Tìm kiếm văn bản tự do (Free Text Search):** Tìm kiếm một thuật ngữ cụ thể trên nhiều trường mà không cần chỉ định tên trường.
    * Ví dụ: `"svc-sql1"` trả về các bản ghi chứa chuỗi này ở bất kỳ trường nào.
* **Toán tử logic:** Hỗ trợ `AND`, `OR`, `NOT` và dấu ngoặc đơn `()` để nhóm biểu thức.
    * Ví dụ: `event.code:4625 AND winlog.event_data.SubStatus:0xC0000072` (Lọc đăng nhập thất bại do tài khoản bị vô hiệu hóa).
* **Toán tử so sánh:** Bao gồm `:`, `:>`, `:>=`, `:<`, `:<=`, và `:!`.
    * Hữu ích để lọc theo phạm vi thời gian hoặc giá trị số cụ thể.
* **Ký tự đại diện (Wildcards):**
    * Ví dụ: `user.name: admin*` tìm các tài khoản bắt đầu bằng "admin" (như administrator, admin123).

---

## 5. Cách xác định dữ liệu khả dụng
Để viết truy vấn hiệu quả, cần biết các trường và giá trị nào có sẵn:

1.  **Sử dụng tính năng Discover & Free Text Search:**
    * Tìm kiếm một giá trị đã biết (ví dụ: "4625") để xem các trường liên quan trong kết quả trả về (như `event.code`, `winlog.event_id`, `@timestamp`).
    * Mở rộng bản ghi để tìm các trường chi tiết hơn (ví dụ: tìm "0xC0000072" để thấy trường `winlog.event_data.SubStatus`).
2.  **Sử dụng Tài liệu của Elastic:**
    * Tham khảo tài liệu về Elastic Common Schema (ECS), Winlogbeat fields, hoặc Filebeat fields để hiểu cấu trúc dữ liệu.

---

## 6. Elastic Common Schema (ECS)
ECS là một bộ từ vựng chung và có thể mở rộng cho các sự kiện và nhật ký trên toàn bộ Elastic Stack, đảm bảo định dạng trường nhất quán.

**Lợi ích của ECS:**
* **Chế độ xem dữ liệu thống nhất:** Dữ liệu từ Windows, mạng, hay cloud đều có thể được tìm kiếm bằng cùng tên trường.
* **Cải thiện hiệu quả tìm kiếm:** Không cần nhớ tên trường riêng biệt cho từng nguồn dữ liệu.
* **Tăng cường khả năng tương quan:** Dễ dàng liên kết các sự kiện từ nhiều nguồn khác nhau (ví dụ: IP từ log tường lửa với log endpoint).
* **Trực quan hóa tốt hơn:** Tạo bảng điều khiển (dashboard) dễ dàng hơn vì các nguồn dữ liệu tuân theo cùng một lược đồ.
* **Khả năng tương tác:** Đảm bảo tương thích với các tính năng nâng cao như Elastic Security và Machine Learning.