# Quy trình Phân loại Cảnh báo (The Triaging Process)

## 1. Phân loại Cảnh báo là gì? (Alert Triaging)
Phân loại cảnh báo là quá trình được thực hiện bởi các chuyên gia phân tích SOC nhằm đánh giá và ưu tiên các cảnh báo bảo mật từ các hệ thống giám sát. Mục tiêu là xác định mức độ đe dọa và tác động tiềm tàng đối với hệ thống và dữ liệu của tổ chức. Quá trình này bao gồm việc xem xét và phân loại các cảnh báo một cách có hệ thống để phân bổ nguồn lực và phản ứng hiệu quả.

### Vai trò của Leo thang (Escalation)
Leo thang là một khía cạnh quan trọng của quá trình phân loại trong môi trường SOC.
* **Định nghĩa:** Quá trình thông báo cho cấp giám sát, đội phản ứng sự cố hoặc các cá nhân có thẩm quyền ra quyết định.
* **Trách nhiệm của chuyên gia phân tích:** Cung cấp thông tin chi tiết về cảnh báo, bao gồm mức độ nghiêm trọng, tác động tiềm tàng và các phát hiện liên quan.
* **Mục đích:** Giúp những người ra quyết định đánh giá tình hình và xác định hướng hành động phù hợp (ví dụ: huy động các nhóm chuyên biệt hoặc nguồn lực bên ngoài).
* **Lợi ích:** Đảm bảo các cảnh báo quan trọng được xử lý kịp thời, tạo điều kiện phối hợp hiệu quả giữa các bên liên quan và tận dụng chuyên môn của cấp quản lý.

---

## 2. Quy trình Phân loại Lý tưởng (The Ideal Triaging Process)
[Image of security alert triaging process flowchart]

Để xử lý cảnh báo hiệu quả, các chuyên gia SOC nên tuân theo một quy trình có cấu trúc bao gồm các bước sau:

### Bước 1: Xem xét Cảnh báo Ban đầu (Initial Alert Review)
* Xem xét kỹ lưỡng cảnh báo bao gồm siêu dữ liệu (metadata), dấu thời gian, IP nguồn/đích, hệ thống bị ảnh hưởng và quy tắc kích hoạt.
* Phân tích các nhật ký liên quan (lưu lượng mạng, hệ thống, ứng dụng) để hiểu ngữ cảnh.

### Bước 2: Phân loại Cảnh báo (Alert Classification)
* Phân loại dựa trên mức độ nghiêm trọng, tác động và tính cấp bách theo hệ thống phân loại của tổ chức.

### Bước 3: Tương quan Cảnh báo (Alert Correlation)
* Đối chiếu với các cảnh báo hoặc sự kiện khác để tìm ra các mẫu, điểm tương đồng hoặc chỉ số xâm phạm (IOCs).
* Truy vấn hệ thống SIEM hoặc quản lý nhật ký để thu thập dữ liệu liên quan.
* Sử dụng nguồn thông tin tình báo mối đe dọa (threat intelligence) để kiểm tra các mẫu tấn công hoặc chữ ký mã độc đã biết.

### Bước 4: Làm giàu Dữ liệu (Enrichment of Alert Data)
* Thu thập thêm thông tin như gói tin mạng (packet captures), bản ghi nhớ (memory dumps) hoặc mẫu tệp.
* Sử dụng công cụ nguồn mở, sandbox hoặc nguồn tình báo bên ngoài để phân tích các tệp, URL hoặc IP đáng ngờ.
* Thăm dò các hệ thống bị ảnh hưởng để tìm kiếm sự bất thường.

### Bước 5: Đánh giá Rủi ro (Risk Assessment)
* Đánh giá rủi ro và tác động đến tài sản quan trọng, dữ liệu nhạy cảm và các yêu cầu tuân thủ.
* Xác định khả năng thành công của cuộc tấn công hoặc khả năng di chuyển ngang (lateral movement) trong mạng.

### Bước 6: Phân tích Ngữ cảnh (Contextual Analysis)
* Xem xét bối cảnh tài sản bị ảnh hưởng và độ nhạy cảm của dữ liệu.
* Đánh giá các biện pháp kiểm soát bảo mật hiện có (tường lửa, IPS) để xem liệu cảnh báo có phải là dấu hiệu của việc né tránh hoặc thất bại kiểm soát hay không.
* Đánh giá các yêu cầu tuân thủ và nghĩa vụ pháp lý.

### Bước 7: Lập Kế hoạch Phản ứng Sự cố (Incident Response Planning)
* Nếu cảnh báo quan trọng, bắt đầu kế hoạch phản ứng: ghi lại chi tiết, chỉ định thành viên đội phản ứng và phối hợp với các nhóm khác.

### Bước 8: Tham vấn với IT Operations (Consultation)
* Thảo luận với bộ phận IT để thu thập thông tin về các thay đổi hệ thống, bảo trì hoặc các vấn đề đã biết có thể gây ra cảnh báo giả (false-positive).
* Ghi lại thông tin chi tiết thu được từ quá trình tham vấn.

### Bước 9: Thực thi Phản ứng (Response Execution)
* Xác định hành động dựa trên kết quả đánh giá và tham vấn.
* Nếu là sự kiện không nguy hiểm, thực hiện các hành động cần thiết mà không cần leo thang.
* Nếu vẫn còn lo ngại về bảo mật, tiến hành các hành động phản ứng sự cố.

### Bước 10: Leo thang (Escalation)
* Xác định các yếu tố kích hoạt leo thang (ví dụ: hệ thống quan trọng bị xâm phạm, tấn công đang diễn ra) theo chính sách tổ chức.
* Tuân thủ quy trình nội bộ để thông báo cho các cấp cao hơn và cung cấp bản tóm tắt toàn diện về cảnh báo.
* Trong một số trường hợp,