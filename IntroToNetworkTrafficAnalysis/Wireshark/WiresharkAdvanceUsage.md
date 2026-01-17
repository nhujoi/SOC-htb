# Wireshark Advanced Usage (Sử dụng Wireshark Nâng cao)

Phần này đi sâu vào các khả năng mạnh mẽ của Wireshark, từ việc theo dõi các cuộc hội thoại TCP đến trích xuất dữ liệu và thông tin xác thực, nhờ vào hệ thống Plugin đa dạng.

## 1. Plugins, Statistics và Analyze Tabs
Các tab **Statistics** (Thống kê) và **Analyze** (Phân tích) cung cấp cái nhìn sâu sắc (insight) về dữ liệu mạng thông qua các plugin có sẵn.

### Statistics Tab
* Cung cấp các báo cáo chi tiết về lưu lượng mạng.
* Hiển thị "Top talkers" (những máy trạm giao tiếp nhiều nhất).
* Phân tích các cuộc hội thoại cụ thể.
* Phân chia dữ liệu theo IP và giao thức.

### Analyze Tab
Cho phép sử dụng các plugin để thực hiện các tác vụ như:
* **Follow TCP streams:** Theo dõi luồng TCP.
* Lọc các loại hội thoại.
* Chuẩn bị bộ lọc gói tin mới.
* **Expert Info:** Xem các thông tin chuyên sâu mà Wireshark tự động tạo ra về lưu lượng.

## 2. Following TCP Streams (Theo dõi luồng TCP)
Wireshark có khả năng ghép nối các gói tin TCP riêng lẻ để tái tạo lại toàn bộ luồng dữ liệu (stream) dưới định dạng dễ đọc. Tính năng này giúp trích xuất dữ liệu (ảnh, văn bản...) từ bất kỳ giao thức nào sử dụng TCP làm cơ chế vận chuyển.

### Cách thực hiện qua GUI:
1.  Nhấp chuột phải vào một gói tin trong luồng muốn xem.
2.  Chọn **Follow** → **TCP**.
3.  Một cửa sổ mới sẽ hiện ra hiển thị toàn bộ nội dung cuộc hội thoại đã được ghép nối.

### Cách thực hiện qua Filter:
* Sử dụng bộ lọc: `tcp.stream eq #` (với # là số thứ tự của luồng) để tìm và theo dõi các cuộc hội thoại cụ thể trong file pcap.
* *Lưu ý:* Khi theo dõi, bạn sẽ thấy quá trình bắt tay 3 bước (Handshake) đầu tiên, sau đó là dữ liệu được truyền tải.

## 3. Extracting Data and Files (Trích xuất Dữ liệu và Tệp)
Wireshark có thể khôi phục file từ các luồng dữ liệu.
* **Điều kiện:** Phải bắt được **toàn bộ** cuộc hội thoại. Nếu thiếu gói tin (incomplete datagram), việc tái tạo sẽ thất bại.

### Cách trích xuất file thông thường:
1.  Dừng quá trình bắt gói tin (Stop capture).
2.  Chọn menu **File** → **Export Objects**.
3.  Chọn định dạng giao thức muốn trích xuất (ví dụ: HTTP, SMB, DICOM...).

## 4. Phân tích và Trích xuất dữ liệu FTP (File Transfer Protocol)
FTP sử dụng TCP để vận chuyển và hoạt động trên 2 cổng chính:
* **Port 20:** Dùng để truyền dữ liệu (Data) giữa máy chủ và máy trạm.
* **Port 21:** Dùng để kiểm soát (Control), gửi các lệnh như login, liệt kê file, upload/download.

### Các bộ lọc FTP quan trọng:
| Bộ lọc | Chức năng | Ứng dụng |
| :--- | :--- | :--- |
| `ftp` | Hiển thị mọi thứ về giao thức FTP. | Xem tổng quan các host/server đang dùng FTP. |
| `ftp.request.command` | Hiển thị lệnh gửi qua kênh kiểm soát (port 21). | Tìm username, password, tên file được yêu cầu. |
| `ftp-data` | Hiển thị dữ liệu truyền qua kênh dữ liệu (port 20). | Bắt nội dung file thực tế để tái tạo lại. |

### Quy trình trích xuất file từ FTP (Thủ công):
1.  **Nhận diện:** Dùng bộ lọc `ftp` để tìm lưu lượng FTP.
2.  **Xác định file:** Dùng `ftp.request.command` để xem ai đã gửi gì và tên file là gì.
3.  **Lọc dữ liệu:** Chọn file muốn lấy, sau đó lọc bằng `ftp-data`.
4.  **Theo dõi luồng:** Chọn gói tin tương ứng với file đó và thực hiện **Follow TCP stream**.
5.  **Lưu file:**
    * Trong cửa sổ Stream, đổi "Show and save data as" sang chế độ **Raw** (Dữ liệu thô).
    * Lưu lại (Save) với tên file gốc.
6.  **Kiểm tra:** Xác thực lại định dạng file vừa trích xuất xem có mở được không.