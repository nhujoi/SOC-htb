# Chi tiết: Endpoint Security (An ninh Thiết bị đầu cuối)

 Bài học này giới thiệu các biện pháp phòng thủ cơ bản cho thiết bị đầu cuối để bảo vệ hệ thống khỏi các cuộc tấn công.

## 1. Host Intrusion Detection Systems (HIDS - Hệ thống Phát hiện Xâm nhập Máy chủ)
*  **Định nghĩa:** HIDS là phần mềm được cài đặt trên thiết bị đầu cuối, cho phép phát hiện các hoạt động đáng ngờ hoặc độc hại.
*  **Cơ chế:** Sử dụng các quy tắc (rules) để kiểm tra hoạt động xem có khớp với các mẫu độc hại đã biết hay không.
*  **Chức năng chính:** Chỉ tạo ra các cảnh báo (alerts) để các chuyên gia phân tích con người điều tra thêm (thông qua giao diện HIDS hoặc đẩy về hệ thống SIEM).

## 2. Host Intrusion Prevention Systems (HIPS - Hệ thống Ngăn chặn Xâm nhập Máy chủ)
*  **Định nghĩa:** HIPS là phần mềm cài trên thiết bị đầu cuối, hoạt động tương tự như HIDS nhưng có khả năng thực hiện các hành động tự động.
*  **Sự khác biệt:** Thay vì chỉ cảnh báo, HIPS có thể tự chủ thực hiện hành động phòng thủ khi phát hiện hoạt động độc hại.
*  **Các hành động phòng thủ:** Các quy tắc của HIPS chứa các hành động cụ thể như ngắt kết nối đến trang web/IP, xóa tệp độc hại, hoặc tạo cảnh báo.

## 3. Anti-Virus Solutions (Giải pháp Diệt Virus - AV)
 AV nên được triển khai trên tất cả các thiết bị đầu cuối (máy tính để bàn, laptop, máy chủ) để phát hiện và loại bỏ mã độc đã biết. Có hai loại chính:

### a. Signature-based (Dựa trên chữ ký)
*  Sử dụng các mẫu hoạt động cụ thể (chữ ký) để nhận diện mã độc đã được ghi nhận trước đó.
*  Hành động: Xóa tệp, tạo cảnh báo hoặc cách ly mã độc.
*  **Hạn chế:** Nếu nhà cung cấp AV chưa có chữ ký của một loại mã độc mới, phần mềm sẽ không phát hiện được và mã độc có thể thực thi thành công.

### b. Behavior-based (Dựa trên hành vi)
*  Đây là loại AV phi truyền thống, hoạt động bằng cách xác định các hành vi đáng ngờ.
*  Cơ chế: Tạo ra một mức cơ sở (baseline) cho các hoạt động "bình thường" và xác định các sai lệch hoặc bất thường không phù hợp với mức cơ sở đó.

## 4. Event Monitoring (Giám sát Sự kiện)
*  Các thiết bị đầu cuối có thể được cấu hình để gửi nhật ký (logs) đến một vị trí tập trung là nền tảng SIEM.
*  **Quy trình:** Dữ liệu được tổng hợp, chuẩn hóa và đối chiếu với các quy tắc để phát hiện/gắn cờ các hoạt động bất thường cho chuyên gia phân tích.
* **Ứng dụng:** Nếu một thiết bị (desktop, server...) bắt đầu hoạt động bất thường, SIEM sẽ ghi nhận và tạo cảnh báo. Syslog thường được sử dụng để đạt được cấp độ ghi nhật ký này.

## 5. Endpoint Detection & Response (EDR - Phát hiện và Phản ứng Điểm cuối)
*  **Định nghĩa:** Các tác nhân (agents) EDR là phần mềm nằm âm thầm trên thiết bị đầu cuối, cung cấp khả năng ghi nhật ký, giám sát và phản ứng.
*  **Điều tra:** Cho phép chuyên gia phân tích đăng nhập và xem chính xác các tiến trình đang chạy, các trang web đã truy cập, tin nhắn đã gửi và chương trình đã chạy.
*  **Mối đe dọa nội bộ:** EDR rất hữu ích để giám sát các mối đe dọa nội bộ bằng cách theo dõi sát sao hành động của người dùng cụ thể.

## 6. Vulnerability Scanning (Quét Lỗ hổng)
 Các đợt quét định kỳ nên được thực hiện để phát hiện cấu hình sai, lỗi bảo mật và lỗ hổng có thể bị khai thác.

### Phân loại theo vị trí:
*  **External scans (Quét bên ngoài):** Thường thực hiện bởi máy quét trên đám mây, cung cấp "góc nhìn của kẻ tấn công" đối với các hệ thống hướng ra internet.
*  **Internal scans (Quét bên trong):** Cung cấp cái nhìn toàn diện hơn về tình trạng bảo mật nội bộ.

### Phân loại theo quyền truy cập:
*  **Credentialed (Có xác thực):** Máy quét đăng nhập vào hệ thống với quyền cao, thu thập nhiều thông tin chi tiết về cấu hình, phiên bản phần mềm.
*  **Non-credentialed (Không xác thực):** Cung cấp góc nhìn thực tế của kẻ tấn công, giúp ưu tiên xử lý các lỗ hổng có khả năng bị khai thác cao nhất.

## 7. Compliance Scanning (Quét Tuân thủ)
*  Một số khung tuân thủ yêu cầu thiết bị đầu cuối phải đáp ứng tiêu chuẩn bảo mật tối thiểu.
*  Các máy quét lỗ hổng thường có các hồ sơ (profiles) hoặc cấu hình cài sẵn để kiểm tra xem hệ thống có đáp ứng các yêu cầu cụ thể của khung tuân thủ hay không.