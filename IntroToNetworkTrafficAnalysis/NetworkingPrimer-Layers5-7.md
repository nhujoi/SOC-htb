# Tóm tắt Chi tiết: Kiến thức Mạng Cơ bản - Lớp 5-7 (Giao thức Ứng dụng)

Phần này xem xét các giao thức lớp ứng dụng (Application Layer) phổ biến giúp duy trì kết nối mạng và truyền tải dữ liệu giữa các máy chủ.

## 1. HTTP (Hypertext Transfer Protocol)
* **Định nghĩa:** Giao thức lớp ứng dụng phi trạng thái (stateless), truyền dữ liệu dạng văn bản rõ (clear text) giữa client và server.
* **Hoạt động:** Sử dụng **TCP** làm giao thức vận chuyển, thường chạy trên cổng **80** hoặc **8000**. Đôi khi có thể dùng cổng khác hoặc UDP.
* **Quy trình:** Client gửi Request -> Server gửi Response (HTML, ảnh, video...).

### Các phương thức HTTP (HTTP Methods)
Các hành động cụ thể khi yêu cầu một tài nguyên (URI):

| Phương thức | Trạng thái | Mô tả |
| :--- | :--- | :--- |
| **HEAD** | **Bắt buộc** | Giống GET nhưng không trả về phần thân (body). Dùng để kiểm tra trạng thái server. An toàn. |
| **GET** | **Bắt buộc** | Phổ biến nhất. Yêu cầu lấy thông tin/nội dung từ server (ví dụ: lấy trang index.html). |
| **POST** | Tùy chọn | Gửi dữ liệu lên server (ví dụ: đăng bài Facebook, gửi biểu mẫu). Tạo các thực thể con (child entities) tại URI. |
| **PUT** | Tùy chọn | Đặt dữ liệu vào URI chỉ định. Nếu chưa có -> Tạo mới. Nếu có rồi -> Cập nhật/Ghi đè. (Tạo hoặc cập nhật đối tượng). |
| **DELETE** | Tùy chọn | Xóa đối tượng tại URI chỉ định. |
| **TRACE** | Tùy chọn | Server phản hồi lại chính yêu cầu đã nhận. Dùng để chẩn đoán từ xa. |
| **OPTIONS** | Tùy chọn | Hỏi server xem nó hỗ trợ những phương thức nào. Không cần tải dữ liệu về. |
| **CONNECT** | Tùy chọn | Dùng cho Proxy/Firewall để tạo đường hầm (tunneling) qua HTTP (ví dụ: SSL tunnels). |

*Lưu ý:* GET và HEAD là bắt buộc phải có trong mọi triển khai chuẩn. Các phương thức còn lại có thể bị tắt tùy vào cấu hình (ví dụ: trang web chỉ đọc).

---

## 2. HTTPS (HTTP Secure)
* **Định nghĩa:** Là phiên bản bảo mật của HTTP, sử dụng **TLS (Transport Layer Security)** hoặc SSL (cũ hơn) để mã hóa.
* **Cổng:** Sử dụng cổng **443** hoặc **8443**.
* **Lợi ích:**
    * Mã hóa toàn bộ cuộc hội thoại (không chỉ dữ liệu).
    * Chống lại tấn công Man-in-the-middle (nghe lén).
    * Bảo vệ giao dịch ngân hàng, tìm kiếm, dữ liệu cá nhân.

### Quy trình bắt tay TLS (TLS Handshake)
Mặc dù dùng TCP, HTTPS cần thêm bước bắt tay TLS để thiết lập bảo mật:
1.  **Client Hello:** Client gửi yêu cầu kết nối an toàn, kèm theo các thông số (phiên bản TLS, thuật toán mã hóa hỗ trợ...).
2.  **Server Hello:** Server đồng ý các thông số, gửi chứng chỉ số (x.509 Certificate).
3.  **Trao đổi khóa:** Hai bên trao đổi các tham số mật mã để tạo ra một "premaster secret".
4.  **Tạo khóa chung:** Từ premaster secret và các giá trị ngẫu nhiên, tạo ra "master secret" dùng chung.
5.  **Xác thực & Kết thúc:** Hai bên xác nhận đã tính toán ra cùng một tham số bảo mật và phiên bắt tay không bị giả mạo.
6.  **Truyền dữ liệu:** Mọi dữ liệu sau đó được mã hóa và gửi đi dưới dạng "TLS Application Data".



---

## 3. FTP (File Transfer Protocol)
* **Định nghĩa:** Giao thức truyền tải tập tin nhanh chóng giữa các thiết bị.
* **Bảo mật:** Vốn là giao thức **không an toàn** (insecure). Ngày nay người dùng chuyển sang **SFTP** (Secure FTP) và trình duyệt web đã bỏ hỗ trợ FTP từ 2020.
* **Cổng & Hoạt động:** Khác biệt vì sử dụng **2 cổng cùng lúc**:
    * **Port 21 (Command):** Dùng để gửi lệnh điều khiển phiên.
    * **Port 20 (Data):** Dùng để truyền dữ liệu thực tế.

### Chế độ hoạt động (Modes)
1.  **Active (Chủ động - Mặc định):** Client báo cho Server biết cổng nào để gửi dữ liệu về (Lệnh PORT).
2.  **Passive (Bị động):** Dùng khi Client nằm sau Firewall/NAT. Client gửi lệnh `PASV`, Server sẽ báo lại IP và Cổng để Client chủ động kết nối tới lấy dữ liệu.

### Các lệnh FTP phổ biến
| Lệnh | Mô tả |
| :--- | :--- |
| **USER / PASS** | Gửi tên đăng nhập và mật khẩu. |
| **PORT** | Đổi cổng dữ liệu (Active mode). |
| **PASV** | Chuyển sang chế độ Passive. |
| **LIST / PWD / CWD** | Liệt kê file / In đường dẫn hiện tại / Đổi thư mục. |
| **RETR** | Tải file về (Retrieve). |
| **QUIT** | Kết thúc phiên. |

---

## 4. SMB (Server Message Block)
* **Định nghĩa:** Giao thức chia sẻ tài nguyên (máy in, file, ổ đĩa) phổ biến trong môi trường **Windows Enterprise**.
* **Đặc điểm:**
    * Hướng kết nối (Connection-oriented), yêu cầu xác thực người dùng.
    * Là mục tiêu hấp dẫn của kẻ tấn công để di chuyển ngang hàng (lateral movement) trong mạng.
* **Cổng & Vận chuyển:**
    * **Hiện đại:** Chạy trực tiếp trên TCP cổng **445**.
    * **Cũ (NetBIOS):** Chạy trên UDP 137, 138 và TCP 139.
    * **Mới nhất:** Hỗ trợ cả giao thức QUIC.

### Phân tích gói tin SMB (SMB On The Wire)
* Vẫn tuân thủ quy trình bắt tay 3 bước của TCP.
* **Dấu hiệu đáng ngờ:**
    * Một hoặc hai lỗi đăng nhập là bình thường.
    * Một cụm lớn (cluster) các lỗi xác thực liên tiếp: Dấu hiệu tấn công Brute-force hoặc dò mật khẩu.
    * Host truy cập vào các file share bất thường trên các host khác (không phải Server): Dấu hiệu di chuyển ngang hàng hoặc thu thập thông tin trái phép.