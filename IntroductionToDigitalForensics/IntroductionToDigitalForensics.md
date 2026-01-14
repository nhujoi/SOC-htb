# Tóm tắt: Giới thiệu về Pháp y Kỹ thuật số (Digital Forensics)

**Lưu ý về phạm vi:**
* Module này cung cấp nền tảng vững chắc cho các chuyên viên phân tích SOC (SOC Analysts), không phải là chương trình toàn diện về mọi mặt của lĩnh vực.
* Trọng tâm chính: Phân tích các hoạt động độc hại trong môi trường **Windows**.

## 1. Định nghĩa & Mục tiêu
**Digital Forensics (Pháp y kỹ thuật số)** là một nhánh chuyên sâu của an ninh mạng, bao gồm việc thu thập, bảo quản, phân tích và trình bày bằng chứng kỹ thuật số để điều tra các sự cố mạng và tội phạm.

* **Đối tượng:** Máy tính, máy chủ, thiết bị di động, mạng, thiết bị lưu trữ.
* **Mục tiêu:**
    * Tái hiện lại dòng thời gian (timeline) sự kiện.
    * Xác định các hoạt động độc hại.
    * Đánh giá mức độ ảnh hưởng.
    * Cung cấp bằng chứng cho các thủ tục pháp lý.
* **Vai trò:** Là phần không thể thiếu của quy trình Ứng phó sự cố (Incident Response - IR).

## 2. Các khái niệm chính (Key Concepts)

### Bằng chứng điện tử (Electronic Evidence)
* Bao gồm: Tệp tin, email, log, cơ sở dữ liệu, lưu lượng mạng...
* Nguồn: Máy tính, thiết bị di động, cloud, server.

### Bảo quản bằng chứng (Preservation of Evidence)
* Đảm bảo tính toàn vẹn và xác thực của dữ liệu.
* Thiết lập **Chuỗi hành trình chứng cứ (Chain of Custody)**.
* Ngăn chặn mọi sự thay đổi vô ý đối với dữ liệu gốc.

### Quy trình pháp y (Forensic Process)
Quy trình tiêu chuẩn gồm 5 giai đoạn:
1.  **Identification (Nhận diện):** Xác định nguồn bằng chứng tiềm năng.
2.  **Collection (Thu thập):** Thu thập dữ liệu bằng phương pháp chuẩn pháp y.
3.  **Examination (Kiểm tra):** Phân tích dữ liệu đã thu thập để tìm thông tin liên quan.
4.  **Analysis (Phân tích):** Giải thích dữ liệu để đưa ra kết luận về sự cố.
5.  **Presentation (Trình bày):** Báo cáo kết quả một cách rõ ràng, dễ hiểu.



## 3. Các trường hợp áp dụng
* Điều tra tội phạm mạng (hack, lừa đảo, trộm cắp dữ liệu).
* Trộm cắp sở hữu trí tuệ.
* Điều tra hành vi sai trái của nhân viên.
* Vi phạm dữ liệu ảnh hưởng đến tổ chức.
* Hỗ trợ kiện tụng pháp lý.

## 4. Các bước cơ bản của điều tra pháp y
1.  **Tạo ảnh pháp y (Create a Forensic Image):** Sao chép dữ liệu bit-by-bit.
2.  **Ghi lại trạng thái hệ thống (Document the System's State):** Chụp ảnh, ghi chú hiện trạng.
3.  **Nhận diện & Bảo quản bằng chứng.**
4.  **Phân tích bằng chứng.**
5.  **Phân tích dòng thời gian (Timeline Analysis).**
6.  **Xác định các chỉ số thỏa hiệp (Identify IOCs).**
7.  **Báo cáo và lập tài liệu (Report and Documentation).**

## 5. Tầm quan trọng của Digital Forensics đối với SOC Analysts

SOC là tuyến phòng thủ đầu tiên, và Digital Forensics hỗ trợ SOC ở các khía cạnh sau:

* **Phân tích hậu sự cố (Post-mortem):** Cung cấp cái nhìn chi tiết sau khi sự cố xảy ra. Giúp truy vết kẻ tấn công, hiểu phương pháp, động cơ và danh tính của chúng.
* **Tốc độ xử lý:** Trong sự cố, công cụ forensics giúp sàng lọc dữ liệu lớn nhanh chóng để xác định thời điểm bị xâm nhập và kỹ thuật tấn công, từ đó giúp khoanh vùng và ngăn chặn thiệt hại nhanh hơn.
* **Giá trị pháp lý:** Cung cấp bằng chứng có giá trị trước tòa (được ghi nhật ký, băm (hash), đóng dấu thời gian cẩn thận) trong trường hợp kiện tụng.
* **Săn tìm mối đe dọa chủ động (Proactive Threat Hunting):** Thay vì chỉ phản ứng với cảnh báo, SOC dùng kiến thức forensics để chủ động tìm kiếm các dấu hiệu xâm nhập (IoCs, TTPs) trong hệ thống.
* **Cải thiện chiến lược Ứng phó sự cố:** Hiểu rõ toàn bộ phạm vi tấn công giúp điều chỉnh phản ứng phù hợp, đảm bảo xử lý triệt để, tránh việc kẻ tấn công quay lại.
* **Học tập liên tục:** Mỗi sự cố được phân tích là một bài học giúp nâng cao năng lực của đội ngũ SOC và củng cố hệ thống phòng thủ.

**Kết luận:** Digital Forensics không chỉ là biện pháp phản ứng (reactive) mà là công cụ chủ động (proactive) giúp tăng cường sức mạnh cho SOC.