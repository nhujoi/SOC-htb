# Phát triển trường hợp sử dụng SIEM (SIEM Use Case Development)

## 1. Trường hợp sử dụng SIEM là gì? (SIEM Use Case)
Việc sử dụng các Use Case trong SIEM là một khía cạnh cơ bản để xây dựng chiến lược an ninh mạng mạnh mẽ, cho phép xác định và phát hiện hiệu quả các sự cố bảo mật tiềm ẩn.

* **Định nghĩa:** Use case được thiết kế để minh họa các tình huống cụ thể nơi một sản phẩm hoặc dịch vụ có thể được áp dụng.
* **Phạm vi:** Chúng có thể bao gồm từ các kịch bản chung (như nỗ lực đăng nhập thất bại) đến các kịch bản phức tạp hơn (như phát hiện bùng phát ransomware).
* **Ví dụ minh họa:**
    * Nếu người dùng có 10 lần thử xác thực thất bại liên tiếp (do quên mật khẩu hoặc bị tấn công brute force), SIEM sẽ tương quan các sự kiện này thành một sự kiện duy nhất.
    * Hệ thống sẽ kích hoạt cảnh báo cho đội ngũ SOC theo danh mục "brute force", và đội SOC sẽ chịu trách nhiệm thực hiện hành động thích hợp dựa trên dữ liệu nhật ký đó.

## 2. Vòng đời phát triển Use Case (Lifecycle)
Quá trình phát triển một use case cần trải qua các giai đoạn quan trọng sau:

1.  **Yêu cầu (Requirements):** Hiểu rõ mục đích hoặc sự cần thiết của use case, xác định kịch bản cụ thể cần cảnh báo (ví dụ: cảnh báo sau 10 lần đăng nhập lỗi trong 4 phút).
2.  **Điểm dữ liệu (Data Points):** Xác định tất cả các nguồn dữ liệu nơi tài khoản người dùng có thể đăng nhập (Windows, Linux, máy chủ, ứng dụng...) và thu thập thông tin về nguồn tạo nhật ký.
3.  **Xác thực nhật ký (Log Validation):** Xác minh rằng nhật ký chứa tất cả thông tin quan trọng (người dùng, dấu thời gian, nguồn, đích...) và đảm bảo nhật ký được nhận trong các sự kiện xác thực khác nhau (VPN, OWA, web...).
4.  **Thiết kế và Triển khai (Design and Implementation):** Xác định các điều kiện kích hoạt cảnh báo dựa trên ba tham số chính: Điều kiện (Condition), Tổng hợp (Aggregation), và Mức độ ưu tiên (Priority).
5.  **Tài liệu hóa (Documentation):** Tạo Quy trình vận hành tiêu chuẩn (SOP) chi tiết các quy trình mà nhà phân tích phải tuân theo, bao gồm ma trận leo thang và thông tin báo cáo.
6.  **Triển khai (Onboarding):** Bắt đầu từ giai đoạn phát triển, xác định và khắc phục các lỗ hổng để giảm thiểu cảnh báo giả (false positives) trước khi chuyển sang môi trường sản xuất.
7.  **Cập nhật định kỳ/Tinh chỉnh (Periodic Update/Fine-tuning):** Nhận phản hồi thường xuyên từ các nhà phân tích và duy trì các quy tắc tương quan cập nhật (ví dụ: whitelisting) để đảm bảo tính hiệu quả.

## 3. Quy trình xây dựng Use Case
Để xây dựng use case hiệu quả, cần thực hiện các bước sau:
* Hiểu rõ nhu cầu, rủi ro và thiết lập cảnh báo giám sát cho tất cả các hệ thống cần thiết.
* Xác định mức độ ưu tiên, tác động và ánh xạ cảnh báo tới chuỗi tiêu diệt (kill chain) hoặc khung MITRE.
* Thiết lập Thời gian phát hiện (TTD) và Thời gian phản hồi (TTR) để đánh giá hiệu quả của SIEM và hiệu suất của nhà phân tích.
* Tạo SOP để quản lý cảnh báo và Kế hoạch phản ứng sự cố (IRP) cho các sự cố dương tính thật.
* Thiết lập Thỏa thuận cấp độ dịch vụ (SLA) và Thỏa thuận cấp độ vận hành (OLA) giữa các nhóm.
* Thực hiện quy trình kiểm toán việc quản lý cảnh báo và tạo tài liệu cơ sở kiến thức (knowledge base).

## 4. Ví dụ thực tế (Sử dụng Elastic Stack)

### Ví dụ 1: Microsoft Build Engine được khởi động bởi ứng dụng Office
* **Bối cảnh:** MSBuild có thể bị kẻ tấn công khai thác để chèn mã độc vào tệp cấu hình. Hành vi đáng ngờ là khi trình duyệt web hoặc ứng dụng Microsoft Office (Excel, Word) khởi chạy MSBuild.
* **Đánh giá rủi ro:** Đây là kỹ thuật "Living-off-the-land binaries" (LoLBins), được xếp loại rủi ro CAO (HIGH severity).
* **Ánh xạ MITRE:**
    * Chiến thuật: Defense Evasion (TA0005) và Execution (TA0002).
    * Kỹ thuật: Trusted Developer Utilities Proxy Execution (T1127) và Sub-technique T1127.001.
* **Xử lý:** Nhà phân tích cần kiểm tra tên quy trình cha (process.parent.name), hoạt động của người dùng trong vòng +/- 2 ngày và kiểm tra nhật ký hệ thống/antivirus.
* **Tinh chỉnh:** Loại trừ các tên quy trình cha hợp lệ (whitelisting) để tránh dương tính giả, vì MSBuild là công cụ phổ biến của lập trình viên nhưng bất thường với người dùng khác.

### Ví dụ 2: MSBuild thực hiện kết nối mạng
* **Bối cảnh:** Máy tính cố gắng giao tiếp ra bên ngoài với địa chỉ IP từ xa hoặc độc hại, và quy trình thực hiện kết nối đó là MsBuild.exe.
* **Đánh giá rủi ro:** Kẻ tấn công dùng MSBuild để thực thi mã và trốn tránh phát hiện.
* **Mức độ ưu tiên:** TRUNG BÌNH (MEDIUM), vì MSBuild cũng có thể kết nối đến các IP hợp lệ (như cập nhật Microsoft), dễ gây ra dương tính giả nếu không có thông tin tình báo mối đe dọa tốt.
* **Ánh xạ MITRE:** Thuộc chiến thuật Execution (TA0002).
* **Xử lý:** Tập trung vào hành động sự kiện (event.action), địa chỉ IP và danh tiếng của IP đó.