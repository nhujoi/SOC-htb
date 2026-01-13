# Quy trình Săn tìm Mối đe dọa (The Threat Hunting Process)

Quy trình này là sự kết hợp giữa kỹ thuật và tư duy sáng tạo, được chia thành 8 giai đoạn cốt lõi:

### 1. Thiết lập bối cảnh (Setting the Stage)
* **Mục tiêu:** Lập kế hoạch và chuẩn bị môi trường.
* **Hoạt động:** * Xác định mục tiêu dựa trên hồ sơ đối thủ và yêu cầu kinh doanh.
    * Đảm bảo hệ thống đã bật ghi log (Logging) đầy đủ.
    * Cấu hình các công cụ hỗ trợ như SIEM, EDR, IDS.
    * Nghiên cứu báo cáo tình báo (Threat Intel) và TTPs (Tactics, Techniques, Procedures).

### 2. Đưa ra giả thuyết (Formulating Hypotheses)
* **Mục tiêu:** Tạo ra các dự đoán có căn cứ để định hướng cuộc săn.
* **Nguồn giả thuyết:** Từ thông tin tình báo mới, cảnh báo từ công cụ bảo mật, hoặc trực giác nghề nghiệp.
* **Yêu cầu:** Giả thuyết phải cụ thể và **có thể kiểm chứng được** (Ví dụ: "Nhóm APT X đang khai thác lỗ hổng Y để tạo kênh C2").

### 3. Thiết kế cuộc săn (Designing the Hunt)
* **Mục tiêu:** Xây dựng chiến lược thực thi.
* **Hoạt động:**
    * Xác định nguồn dữ liệu cần phân tích (Log web, traffic mạng, DNS...).
    * Lựa chọn công cụ, kỹ thuật và các chỉ số IoCs (Indicators of Compromise).
    * Viết các truy vấn (queries) hoặc script tùy chỉnh để lọc dữ liệu.

### 4. Thu thập và Kiểm tra dữ liệu (Data Gathering and Examination)
* **Mục tiêu:** Tìm kiếm bằng chứng ủng hộ hoặc bác bỏ giả thuyết.
* **Hoạt động:** * Phân tích log file, lưu lượng mạng và dữ liệu endpoint.
    * Sử dụng các kỹ thuật: Phân tích thống kê, phân tích hành vi hoặc phát hiện dựa trên chữ ký.
    * Đây là quá trình lặp đi lặp lại (iterative), có thể điều chỉnh hướng đi khi có thông tin mới.



### 5. Đánh giá kết quả (Evaluating Findings)
* **Mục tiêu:** Diễn giải các phát hiện.
* **Hoạt động:** * Xác nhận giả thuyết đúng hay sai.
    * Xác định phạm vi ảnh hưởng (các hệ thống bị tác động) và mức độ thiệt hại tiềm tàng.

### 6. Giảm thiểu mối đe dọa (Mitigating Threats)
* **Mục tiêu:** Loại bỏ mối đe dọa và hạn chế thiệt hại.
* **Hoạt động:** * Cách ly hệ thống bị nhiễm.
    * Xóa mã độc, vá lỗ hổng, thay đổi cấu hình bảo mật.
    * Ngăn chặn rò rỉ dữ liệu.

### 7. Sau cuộc săn (After the Hunt)
* **Mục tiêu:** Lưu trữ tài liệu và chia sẻ kiến thức.
* **Hoạt động:** * Cập nhật IoCs vào nền tảng Threat Intel.
    * Cải thiện các quy tắc phát hiện (detection rules) và kịch bản ứng phó sự cố (playbooks).
    * Báo cáo kết quả và phương pháp thực hiện.

### 8. Học hỏi và Cải thiện liên tục (Continuous Learning)
* **Mục tiêu:** Nâng cao năng lực cho các cuộc săn tương lai.
* **Hoạt động:** Đánh giá hiệu quả của công cụ/giả thuyết, cập nhật kiến thức về các kỹ thuật tấn công mới nhất.

---

## Case Study: Săn tìm Mã độc Emotet

| Giai đoạn | Hành động cụ thể với Emotet |
| :--- | :--- |
| **Chuẩn bị** | Nghiên cứu vector lây nhiễm của Emotet (macro trong file Word, link lừa đảo). Xác định các tài sản quan trọng như Email Server. |
| **Giả thuyết** | "Emotet đang sử dụng các tài khoản email bị chiếm quyền để gửi phishing kèm file Word chứa macro độc hại." |
| **Thiết kế** | Thu thập log mail server, log mạng (C2 traffic) và hash file của payload Emotet đã biết. |
| **Thu thập/Phân tích** | Kiểm tra header email, phân tích hành vi file trong sandbox, soi lưu lượng mạng đến các IP C2 đã biết. |
| **Đánh giá** | Xác nhận có hàng loạt email trùng chủ đề/loại file đính kèm, phát hiện kết nối outbound đến IP độc hại. |
| **Giảm thiểu** | Cách ly máy bị nhiễm, reset tài khoản email bị chiếm, chặn các IP C2 trên Firewall. |
| **Sau cuộc săn** | Cập nhật tập luật EDR để bắt các biến thể macro mới của Emotet, đào tạo nhận thức người dùng về phishing. |
| **Học hỏi** | Tích hợp thuật toán Machine Learning để phát hiện sớm các thay đổi trong TTPs của Emotet. |