# Chi tiết: Email Security (An ninh Email)

Bài học này giới thiệu các biện pháp phòng thủ email cơ bản nhằm bảo vệ tổ chức khỏi các cuộc tấn công, đặc biệt nhấn mạnh tầm quan trọng của việc bảo vệ "yếu tố con người".

## 1. Tổng quan về An ninh Email
* **Thực trạng:** Tấn công giả mạo (Phishing) là vectơ tấn công số một để xâm phạm các tổ chức, dẫn đến vi phạm dữ liệu. Dù vậy, an ninh email thường bị xem nhẹ thay vì là ưu tiên hàng đầu.
* **Mục tiêu:** Các email độc hại nhắm vào con người thay vì hệ thống CNTT. Do đó, nhân viên cần được đào tạo để phát hiện và phản ứng với các email lọt qua được các lớp phòng thủ kỹ thuật.

## 2. Spam Filter (Bộ lọc Thư rác)
* **Định nghĩa:** Là phần mềm quét các email đến để tìm các dấu hiệu của thư rác hoặc email độc hại.
* **Chức năng:** Ngăn chặn các email này được chuyển đến hộp thư của nhân viên, giữ cho hộp thư không bị đầy rác hoặc tin nhắn nguy hiểm.
* **Vai trò:** Đây là biện pháp kiểm soát an ninh cơ bản nhưng cốt lõi, đóng vai trò phòng thủ tuyến đầu giúp giảm bớt khối lượng công việc cho các chuyên gia phân tích bảo mật.

## 3. Data Loss Prevention (DLP - Ngăn chặn Thất thoát Dữ liệu)
* **Định nghĩa:** Biện pháp kiểm soát an ninh nhằm ngăn chặn thông tin nhạy cảm (tệp tin, thông tin ngân hàng, thông tin cá nhân - PII) rời khỏi tổ chức trái phép.
* **Phạm vi áp dụng:** Trong mô-đun này, DLP tập trung vào giao tiếp qua email. Hệ thống có thể giám sát:
    * Nội dung thân email (body).
    * Tiêu đề email (headers).
    * Các tệp đính kèm.
* **Cơ chế hoạt động:**
    * Quét các từ khóa cụ thể hoặc sử dụng biểu thức chính quy (regex) để gắn cờ nội dung.
    * Nếu phát hiện thông tin quan trọng sắp bị gửi ra ngoài, email sẽ bị chặn ngay tại cổng email (gateway) và không được gửi đi.
* **Ví dụ thực tế:** Ngăn chặn một nhân viên bất mãn cố gắng gửi tài liệu quan trọng của công ty cho đối thủ cạnh tranh trước khi bị sa thải.

## 4. Email Scanning (Quét Email)
* **Mục tiêu:** Phát hiện các email lừa đảo thường chứa URL độc hại hoặc tệp đính kèm độc hại.
* **Cơ chế:** Các máy quét chuyên dụng sẽ đọc tiêu đề và nội dung email để xác định các chỉ số độc hại (dựa trên chữ ký, tên miền xấu...).
* **Hành động:** Khi phát hiện email đáng ngờ, hệ thống sẽ cách ly (quarantine) email đó để không gửi đến người dùng, đồng thời tạo cảnh báo cho đội ngũ bảo mật điều tra.

## 5. Security Awareness Training (Đào tạo Nhận thức Bảo mật)
* **Yêu cầu:** Nên là chương trình bắt buộc cho nhân viên mới và được thực hiện định kỳ cho toàn bộ nhân viên (thường được quy định bởi các khung tuân thủ).
* **Trọng tâm:** Phishing đóng vai trò lớn trong nội dung đào tạo.
* **Mục đích:**
    * Hướng dẫn nhân viên cách phát hiện các dấu hiệu đáng ngờ.
    * Quy định rõ các bước cần làm khi gặp email lạ (nhắn tin/gọi cho đội bảo mật hoặc chuyển tiếp email đến hộp thư quy định).
* **Tầm quan trọng:** Vì email vẫn có thể lọt qua các biện pháp kiểm soát kỹ thuật, nhân viên đóng vai trò là chốt chặn cuối cùng - họ cần biết không được nhấp vào liên kết hay chạy tệp đính kèm.

## 6. Simulated Phishing (Giả lập Phishing)
* Thường được kết hợp với chương trình đào tạo nhận thức.
* Đội ngũ bảo mật thực hiện các chiến dịch giả lập để kiểm tra nhân viên.
* **Các chỉ số đo lường:**
    * Số lượng nhân viên báo cáo email cho bộ phận bảo mật (Tốt).
    * Số lượng nhân viên đã nhấp vào liên kết độc hại giả định (Cần cải thiện).