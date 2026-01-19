# Disk Forensics (Điều tra số Ổ đĩa)

Sau khi hoàn thành phân tích bộ nhớ (Memory Forensics), bước tiếp theo là chuyển sang **Disk Forensics** (kiểm tra và phân tích ảnh ổ đĩa). Việc tuân thủ quy tắc về độ biến động dữ liệu (order of volatility) là rất quan trọng để không bỏ sót các dấu vết nhỏ nhất.

## 1. Các tính năng cốt lõi của công cụ Disk Forensics
Đối với các đội ứng phó sự cố (Incident Response), dù công cụ là mã nguồn mở hay thương mại, chúng cần phải có các chức năng nổi bật sau:

* **File Structure Insight (Cấu trúc tập tin):**
    * Khả năng hiển thị và điều hướng phân cấp thư mục (hierarchy) của ổ đĩa.
    * Cho phép truy cập nhanh vào các tệp cụ thể, đặc biệt là tại các vị trí hệ thống quan trọng trên máy nghi vấn.
* **Hex Viewer (Trình xem Hex):**
    * Xem dữ liệu thô từng byte dưới dạng thập lục phân (hexadecimal).
    * Cực kỳ cần thiết khi phân tích các mối đe dọa phức tạp như malware được thiết kế riêng hoặc các mã khai thác (exploits).
* **Web Artifacts Analysis (Phân tích dấu vết Web):**
    * Sàng lọc và trình bày dữ liệu hoạt động web của người dùng một cách hiệu quả.
    * Quan trọng để tái hiện chuỗi sự kiện (ví dụ: truy vết nguyên nhân người dùng truy cập vào website độc hại).
* **Email Carving (Trích xuất Email):**
    * Trích xuất và hiển thị dữ liệu email để kết nối các manh mối.
    * Đặc biệt hữu ích trong điều tra mối đe dọa nội bộ (nhân viên xấu hoặc sơ suất).
* **Image Viewer (Trình xem ảnh):**
    * Tích hợp sẵn công cụ xem hình ảnh lưu trên hệ thống để phục vụ kiểm tra chính sách hoặc điều tra sâu hơn.
* **Metadata Analysis (Phân tích siêu dữ liệu):**
    * Cung cấp chi tiết như: Thời gian tạo file (timestamps), giá trị băm (hashes), vị trí lưu trữ.
    * Giúp liên kết các sự kiện (Ví dụ: Khớp thời gian khởi chạy ứng dụng với thời gian cảnh báo malware xuất hiện).

## 2. Giới thiệu về Autopsy
**Autopsy** là một nền tảng pháp y kỹ thuật số thân thiện với người dùng, được xây dựng dựa trên bộ công cụ mã nguồn mở **The Sleuth Kit**.



* **Đặc điểm:**
    * Sở hữu nhiều tính năng mạnh mẽ tương đương các công cụ thương mại.
    * Bao gồm: Phân tích dòng thời gian (timeline), tìm kiếm từ khóa, trích xuất artifact web/email, và lọc kết quả dựa trên danh sách hash độc hại đã biết.

* **Quy trình làm việc trên Autopsy:**
    Sau khi nạp (load) ảnh đĩa và xử lý dữ liệu, các artifact sẽ được tổ chức ngăn nắp ở thanh bên (side panel), cho phép thực hiện:
    1.  **Data Sources:** Khám phá sâu vào các tập tin và thư mục gốc.
    2.  **Web Artifacts:** Kiểm tra dữ liệu trình duyệt.
    3.  **Attached Devices:** Kiểm tra các thiết bị ngoại vi từng kết nối.
    4.  **Recover Deleted Files:** Khôi phục các tập tin đã bị xóa.
    5.  **Keyword Searches:** Tìm kiếm từ khóa thủ công.
    6.  **Keyword Lists:** Sử dụng danh sách từ khóa mục tiêu để tìm kiếm.
    7.  **Timeline Analysis:** Lập bản đồ các sự kiện theo trình tự thời gian.