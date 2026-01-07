# SIEM (Security Information and Event Management)

## 1. SIEM là gì?
**SIEM** (Security Information and Event Management) là nhóm **phần mềm/giải pháp** kết hợp:
- **Quản lý dữ liệu an ninh** (security data management)  
- **Giám sát và quản lý sự kiện an ninh** (security event monitoring/management)

Mục tiêu cốt lõi của SIEM là **thu thập, chuẩn hoá, phân tích và tương quan** (correlate) dữ liệu nhật ký/sự kiện từ nhiều hệ thống để:
- **Đánh giá cảnh báo bảo mật theo thời gian thực**
- **Phát hiện sớm tấn công** (ngay khi xảy ra hoặc trước khi gây hậu quả lớn)
- **Tăng tốc điều tra và ứng phó sự cố**

### Chức năng lõi thường gặp
- **Thu thập và quản trị log** (log collection & management)
- **Phân tích log và dữ liệu bổ sung** từ nhiều nguồn
- Tính năng vận hành SOC:
  - **Quản lý sự cố (incident handling)**
  - **Dashboard/tổng quan trực quan**
  - **Báo cáo và lưu vết (documentation/reporting)**

---

## 2. SIEM hình thành và phát triển như thế nào?
Thuật ngữ “SIEM” xuất hiện khi Gartner đề xuất một khung thông tin an ninh mới, **kết hợp hai công nghệ đi trước**:
- **SIM** (Security Information Management)
- **SEM** (Security Event Management)

Ý tưởng này được nêu trong bài viết Gartner năm 2005 về tăng cường an ninh CNTT thông qua quản lý lỗ hổng.

### SIM (thế hệ đầu)
Được xây dựng từ hệ thống thu thập log truyền thống, tập trung vào:
- **Lưu trữ dài hạn**
- **Phân tích và báo cáo log**
- Có thể **kết hợp log với threat intelligence**

### SEM (thế hệ hai)
Tập trung vào **sự kiện an ninh**, cung cấp:
- **Hợp nhất (consolidation)**
- **Tương quan (correlation)**
- **Thông báo/cảnh báo (notification)**  
cho sự kiện đến từ nhiều thiết bị/công cụ bảo mật và hệ thống, ví dụ:
- Antivirus
- Firewall
- IDS
- Sự kiện xác thực (authentication)
- SNMP traps
- Server, database

### SIEM (kết hợp SIM + SEM)
Các nhà cung cấp dần **hợp nhất** SIM và SEM để tạo ra SIEM: một cách tiếp cận toàn diện cho **thu thập, lưu giữ, phân tích log + sự kiện**, từ đó phát hiện và quản lý đe doạ tốt hơn.

---

## 3. SIEM hoạt động như thế nào?
SIEM vận hành theo chu trình tổng quát:

1. **Thu thập dữ liệu** từ nhiều nguồn:
   - Máy tính/endpoint
   - Thiết bị mạng
   - Server
   - Ứng dụng và hạ tầng khác

2. **Chuẩn hoá và hợp nhất** dữ liệu:
   - Dữ liệu thô (raw logs) được đưa về định dạng “chung” để dễ phân tích
   - Giúp công cụ tương quan hoạt động chính xác hơn

3. **Phân tích và phát hiện**:
   - Chuyên gia an ninh (SOC) rà soát dữ liệu
   - Áp dụng rule/detection logic để nhận diện nguy cơ

4. **Sinh cảnh báo (alerts)**:
   - Cảnh báo thường ngắn gọn, chỉ ra dấu hiệu tấn công hoặc sự kiện đáng ngờ
   - Kênh gửi cảnh báo có thể gồm:
     - Email
     - Pop-up trên console
     - Tin nhắn SMS
     - Gọi/notify tới smartphone

### Vấn đề “quá tải cảnh báo” và nhu cầu tuning
Do hệ thống tạo ra **khối lượng sự kiện cực lớn** (hàng trăm đến hàng ngàn sự kiện/giờ), SIEM có thể tạo **rất nhiều cảnh báo**. Vì vậy, cần:
- **Tinh chỉnh (fine-tuning)** để ưu tiên sự kiện rủi ro cao
- Giảm **false positives** và **alert fatigue**

### SIEM khác IDS/IPS như thế nào?
- SIEM **không thay thế** khả năng ghi log của IDS/IPS
- SIEM **phối hợp** với IDS/IPS bằng cách:
  - Thu nhận log/sự kiện từ nhiều nguồn
  - **Tương quan** để phát hiện chuỗi hành vi hoặc mẫu tấn công
  - Cung cấp góc nhìn toàn cảnh thay vì chỉ theo từng thiết bị đơn lẻ

---

## 4. Yêu cầu nghiệp vụ & các use case chính của SIEM

### 4.1. Tập trung log và chuẩn hoá (Log Aggregation & Normalization)
SIEM giúp tạo **tầm nhìn mối đe doạ** bằng cách tập trung log từ:
- Firewall quan trọng
- CSDL nhạy cảm
- Ứng dụng trọng yếu

Nhờ vậy, SOC có thể:
- Tìm mối liên hệ giữa các sự kiện
- Phát hiện mẫu bất thường (anomaly), xu hướng (trend), chuỗi hành vi (pattern)
- Điều tra sự cố xuyên suốt toàn bộ hạ tầng CNTT

### 4.2. Cảnh báo đe doạ (Threat Alerting)
SIEM cần có khả năng:
- Nhận diện dấu hiệu tấn công trong “biển” dữ liệu
- Phát sinh cảnh báo theo thời gian thực dựa trên:
  - Phân tích nâng cao
  - Threat intelligence

Mục tiêu là để đội an ninh:
- Điều tra nhanh hơn
- Khoanh vùng đúng hơn
- Giảm thời gian phản ứng và giảm tác động sự cố

### 4.3. Ngữ cảnh hoá & hỗ trợ ứng phó (Contextualization & Response)
Chỉ “bắn cảnh báo” là chưa đủ. SIEM cần cung cấp **ngữ cảnh**:
- Ai/đối tượng nào liên quan
- Phần nào của mạng/hệ thống bị ảnh hưởng
- Thời điểm và chuỗi diễn tiến

Ngữ cảnh giúp:
- Phân loại cảnh báo quan trọng
- Lọc bớt nhiễu (tự động hoá lọc theo ngữ cảnh)
- Tập trung vào mối đe doạ thật

Một SIEM “lý tưởng” còn cho phép doanh nghiệp **quản lý/kiểm soát rủi ro ngay trong quá trình điều tra**, ví dụ tạm dừng một số hoạt động để hạn chế lan rộng sự cố.

### 4.4. Tuân thủ (Compliance)
SIEM hỗ trợ đáp ứng các yêu cầu tuân thủ bằng:
- Giám sát thời gian thực và phân tích lưu lượng/sự kiện
- **Báo cáo tự động** và **khả năng audit**

Các quy định thường được nhắc đến:
- PCI DSS
- HIPAA
- GDPR

Doanh nghiệp có thể:
- Tạo báo cáo tuân thủ nhanh, chính xác
- Chứng minh với kiểm toán viên/cơ quan quản lý rằng hệ thống có theo dõi, lưu log và tuân thủ chính sách lưu giữ log

---

## 5. Luồng dữ liệu trong SIEM (Data Flows)
Tóm lược đường đi của dữ liệu trước khi dùng để phân tích:

1. **Ingest/Collect**: SIEM nhận log từ nhiều nguồn (mỗi SIEM có khả năng thu thập khác nhau)
2. **Normalize/Aggregate**: dữ liệu thô được chuyển đổi về định dạng chung để correlation engine hiểu được
3. **Detect/Operate**: SOC dùng dữ liệu đã chuẩn hoá để tạo:
   - Rule phát hiện
   - Dashboard/Visualization
   - Alert
   - Incident

---

## 6. Lợi ích của SIEM
Khi không có SIEM, tổ chức thường gặp:
- Không có **góc nhìn tập trung** về log và sự kiện
- Dễ bỏ sót sự kiện quan trọng
- Tích tụ nhiều sự kiện chưa điều tra

Khi có SIEM được cấu hình tốt:
- **Tăng hiệu quả ứng phó sự cố**
- Có dashboard tập trung và cảnh báo theo:
  - Loại sự kiện
  - Ngưỡng (threshold)
  - Khung thời gian (time window)

### Ví dụ minh hoạ
- Firewall ghi nhận **5 lần đăng nhập sai liên tiếp** dẫn tới khoá tài khoản admin: SIEM giúp tương quan và theo dõi tập trung.
- Web filtering ghi nhận một máy truy cập **website độc hại 100 lần/giờ**: SIEM cho phép quan sát và xử lý trong một giao diện.

### Xu hướng SIEM hiện đại
- Tự phát hiện theo **ngưỡng cấu hình** và theo **khung thời gian**
- Báo cáo tùy biến, tổng hợp thông minh
- SIEM nâng cao tích hợp **AI** để cảnh báo theo:
  - Hành vi (behavior)
  - Mẫu hình (pattern)

### Giá trị kinh doanh
- Phát hiện sớm giúp giảm chi phí sự cố
- Tránh thiệt hại tài chính và uy tín

### Yêu cầu bắt buộc ở ngành bị quản lý chặt
Nhiều tổ chức thuộc Banking/Finance/Insurance/Healthcare thường phải có SIEM (on-prem hoặc cloud) để:
- Chứng minh hệ thống được giám sát và lưu log
- Thực hiện review log
- Tuân thủ chính sách lưu giữ log
- Đáp ứng chuẩn/khung tuân thủ (ví dụ ISO, HIPAA)

---

## 7. Ghi nhớ nhanh
- **SIEM = SIM + SEM**
- 3 bước kỹ thuật dễ nhớ: **Thu thập → Chuẩn hoá → Tương quan/Phát hiện**
- 4 use case trọng tâm: **Log tập trung, Cảnh báo, Ngữ cảnh & ứng phó, Tuân thủ**
- Thành công phụ thuộc nhiều vào: **tuning** (giảm nhiễu, tập trung rủi ro cao)
