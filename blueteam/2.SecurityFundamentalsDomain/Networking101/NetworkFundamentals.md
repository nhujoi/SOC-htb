# Chi tiết: Network Fundamentals (Căn bản về Mạng)

Bài học này cung cấp kiến thức nền tảng về mạng máy tính cho học viên chưa có nhiều kinh nghiệm, bao gồm các giao thức cốt lõi (TCP, UDP, ICMP) và các khái niệm định danh (IP, MAC Address).

## 1. Transmission Control Protocol (TCP)
**Định nghĩa:** TCP là giao thức hướng kết nối (connection-oriented), cho phép hai hệ thống thiết lập kết nối để truyền dữ liệu hai chiều. Nó hoạt động ở tầng Giao vận (Transport layer) trong mô hình OSI.

**Đặc điểm chính:**
* **Độ tin cậy cao:** Mọi dữ liệu bị mất sẽ được phát hiện và tự động sửa chữa.
* **TCP/IP Stack:** Thường được gọi chung với IP vì TCP hầu như luôn dựa trên giao thức Internet (IP).

### Quy trình bắt tay ba bước (Three-way Handshake)
Để giao tiếp, các hệ thống thực hiện quy trình bắt tay. Điều kiện tiên quyết là cả hai hệ thống phải có địa chỉ IP duy nhất và cổng (port) đã được gán/bật.

1.  **SYN:** Máy khách (Client) gửi một gói tin SYN (synchronize) kèm theo một số ngẫu nhiên đến máy chủ (Server). Điều này đảm bảo dữ liệu được gửi đúng thứ tự.
2.  **SYN-ACK:** Máy chủ nhận gói tin và chấp nhận kết nối bằng cách gửi lại gói SYN-ACK. Gói này bao gồm số thứ tự của Client cộng thêm 1, đồng thời Máy chủ cũng truyền số thứ tự riêng của nó.
3.  **ACK:** Máy khách xác nhận việc nhận được SYN-ACK bằng cách gửi gói ACK chứa số thứ tự của Server cộng thêm 1. Tại thời điểm này, kết nối được thiết lập (ESTABLISHED) và dữ liệu bắt đầu được truyền.

---

## 2. User Datagram Protocol (UDP)
**Định nghĩa:** UDP là giao thức cho phép gửi các gói dữ liệu (datagrams) mà không cần thiết lập kết nối trong mạng IP. Nó đóng vai trò trung gian giữa tầng mạng và tầng ứng dụng.

**4 Đặc điểm chính của UDP:**
1.  **Không kết nối (Connectionless):** Dữ liệu được gửi đến IP và cổng đích mà không cần thiết lập kết nối trước. Máy tính nhận không cần phải phản hồi.
2.  **Sử dụng Cổng (Ports):** Giống như TCP, UDP sử dụng các cổng (từ 0 đến 1023 cho các dịch vụ cố định) để chuyển dữ liệu đến đúng ứng dụng đích.
3.  **Tốc độ nhanh, không độ trễ:** Do không mất thời gian thiết lập kết nối và việc mất gói tin lẻ tẻ không làm dừng toàn bộ quá trình truyền (khác với TCP sẽ yêu cầu gửi lại gói tin bị mất).
4.  **Không đảm bảo bảo mật và toàn vẹn:** Không có xác thực lẫn nhau, không đảm bảo dữ liệu đến đủ hoặc đúng thứ tự. Các dịch vụ sử dụng UDP phải tự xây dựng cơ chế sửa lỗi hoặc bảo vệ riêng.

---

## 3. Internet Control Message Protocol (ICMP)
**Định nghĩa:** ICMP là giao thức thuộc tầng Internet (Internet layer), được sử dụng bởi các thiết bị mạng (như Router) để chẩn đoán các vấn đề giao tiếp mạng.

**Chức năng:**
* Xác định xem dữ liệu có đến được đích kịp thời hay không.
* Công cụ phổ biến nhất sử dụng ICMP là lệnh `ping` (dùng để kiểm tra khả năng tiếp cận của một IP hoặc tên miền).

---

## 4. IP Addresses (Địa chỉ IP)
**Định nghĩa:** Địa chỉ IP cung cấp danh tính cho một thiết bị trên mạng, tương tự như địa chỉ nhà thực tế. Máy tính sử dụng máy chủ DNS để tra cứu tên miền (Hostname) và tìm ra địa chỉ IP tương ứng.

### Private IP Addresses (IP Riêng)
* **Mục đích:** Sử dụng bên trong một mạng nội bộ (như mạng gia đình, văn phòng) để các thiết bị (tablet, camera, PC) giao tiếp với Router và với nhau.
* **Các dải địa chỉ Private IP:**
    * `192.168.0.0` - `192.168.255.255` (65,536 địa chỉ)
    * `172.16.0.0` - `172.31.255.255` (1,048,576 địa chỉ)
    * `10.0.0.0` - `10.255.255.255` (16,777,216 địa chỉ)

### Public IP Addresses (IP Công cộng)
* **Mục đích:** Được gán bởi nhà cung cấp dịch vụ Internet (ISP). Đây là địa chỉ chính giúp mạng gia đình hoặc doanh nghiệp giao tiếp với thế giới bên ngoài (Internet).

### Static vs. Dynamic IPs (Tĩnh và Động)
* **Dynamic IP:** Địa chỉ thay đổi, được gán tự động bởi máy chủ DHCP.
* **Static IP:** Địa chỉ cố định không thay đổi, được gán thủ công (thường dùng khi thiết bị không hỗ trợ DHCP hoặc cần cố định).

---

## 5. MAC Addresses (Địa chỉ MAC)
**Định nghĩa:** Media Access Control (MAC) là mã định danh phần cứng duy nhất xác định từng thiết bị trên mạng.

**Đặc điểm:**
* Được nhà sản xuất ghi cứng vào card mạng (Ethernet hoặc Wi-Fi).
* Về lý thuyết không thể thay đổi, nhưng kẻ tấn công có thể giả mạo (spoofing).
* **Cấu trúc:** Gồm 6 cặp số thập lục phân (hexadecimal), ngăn cách bởi dấu hai chấm.
    * Ví dụ: `00:00:83:b1:c0:8e`
* **Cách xem trên Windows:** Tìm kiếm "Network Status" -> Chọn "View your network properties".