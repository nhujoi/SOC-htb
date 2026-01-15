# Tóm tắt Chi tiết: Căn bản về Tcpdump (Tcpdump Fundamentals)

**Tcpdump** là một công cụ phân tích gói tin dòng lệnh (command-line packet sniffer) mạnh mẽ, được sử dụng để bắt và giải mã các khung dữ liệu (data frames) trực tiếp từ giao diện mạng hoặc từ file.

## 1. Giới thiệu & Cài đặt
* **Đặc điểm:**
    * Chạy trên hầu hết các hệ điều hành Unix/Linux (Windows có bản port cũ là WinDump, hoặc dùng qua WSL).
    * Không cần giao diện đồ họa (GUI), hoạt động tốt qua SSH/Terminal.
    * Sử dụng thư viện `pcap` và `libpcap` để bắt gói tin.
    * Đặt card mạng ở chế độ **Promiscuous mode** để nghe toàn bộ traffic trong mạng LAN (không chỉ traffic gửi đến máy mình).
* **Quyền hạn:** Yêu cầu quyền **Root** hoặc **Sudo** để truy cập trực tiếp vào phần cứng mạng.
* **Cài đặt & Kiểm tra:**
    * Kiểm tra vị trí: `which tcpdump` (thường ở `/usr/sbin/tcpdump`).
    * Cài đặt (nếu chưa có): `sudo apt install tcpdump`.
    * Kiểm tra phiên bản: `sudo tcpdump --version`.

---

## 2. Các Tùy chọn Bắt gói tin Cơ bản (Basic Capture Options)
Tcpdump có rất nhiều "công tắc" (switches) để điều chỉnh việc bắt gói tin. Các switch này có thể kết hợp với nhau.

| Switch | Chức năng | Ví dụ |
| :--- | :--- | :--- |
| **-D** | Liệt kê các giao diện mạng khả dụng để bắt gói tin. | `tcpdump -D` |
| **-i** | Chọn giao diện cụ thể để bắt gói tin. | `-i eth0` |
| **-n** | Không phân giải tên miền (DNS) và tên cổng (Service). Hiển thị IP và số cổng giúp chạy nhanh hơn. | |
| **-e** | Hiển thị Ethernet header (địa chỉ MAC) cùng với dữ liệu lớp trên. | |
| **-X** | Hiển thị nội dung gói tin dưới dạng Hex và ASCII. | |
| **-XX** | Giống -X nhưng bao gồm cả Ethernet header. | |
| **-v, -vv, -vvv** | Tăng mức độ chi tiết (verbosity) của thông tin hiển thị. | |
| **-c** | Bắt một số lượng gói tin cụ thể rồi tự động dừng. | `-c 10` (bắt 10 gói) |
| **-s** | Định nghĩa kích thước gói tin cần bắt (snapshot length). | |
| **-S** | Hiển thị số thứ tự (Sequence number) tuyệt đối thay vì tương đối. | |
| **-q** | In ít thông tin giao thức hơn (Quiet mode). | |
| **-w** | Ghi kết quả bắt được vào file `.pcap`. | `-w capture.pcap` |
| **-r** | Đọc dữ liệu từ file `.pcap`. | `-r capture.pcap` |
| **-l** | Make stdout line buffered. | `tcpdump -l \| tee dat` |

*Mẹo:* Dùng lệnh `man tcpdump` để xem hướng dẫn đầy đủ.

---

## 3. Ví dụ Sử dụng & Phân tích (Usage Examples)

### Liệt kê & Chọn giao diện
* Lệnh: `sudo tcpdump -D` -> Hiển thị danh sách (eth0, any, lo, bluetooth...).
* Lệnh: `sudo tcpdump -i eth0` -> Bắt đầu bắt gói tin trên cổng `eth0`.

### Tắt phân giải tên (-n / -nn)
* Mặc định Tcpdump cố gắng dịch IP sang tên miền và Port sang tên dịch vụ (ví dụ: 80 -> http).
* Sử dụng `-nn`: Giữ nguyên dạng số `IP.Port` (ví dụ: `172.16.146.2.48402`). Giúp nhìn rõ port nguồn/đích thực tế.

### Hiển thị Ethernet Header (-e)
* Thay vì bắt đầu bằng IP Header, dòng log sẽ hiển thị **MAC nguồn > MAC đích** và loại Ethernet (IPv4...).

### Hiển thị nội dung Hex & ASCII (-X)
* Rất hữu ích để soi nội dung gói tin (payload).
* Bên trái là mã Hex, bên phải là ký tự ASCII (văn bản rõ). Giúp đọc được các thông tin không mã hóa.

### Kết hợp các Switch (-nnvXX)
* Lệnh: `sudo tcpdump -i eth0 -nnvXX`
* Kết quả: Hiển thị chi tiết tối đa, không phân giải tên, bao gồm cả Ethernet header và nội dung Hex/ASCII của toàn bộ gói tin.

---

## 4. Giải mã Kết quả Tcpdump (Tcpdump Output Breakdown)

Khi nhìn vào một dòng log của Tcpdump, thông tin được sắp xếp theo cấu trúc nhất định.
![alt text](image.png)

| Trường (Field) | Màu sắc (trong ví dụ) | Mô tả |
| :--- | :--- | :--- |
| **Timestamp** | Vàng | Thời gian bắt được gói tin. |
| **Protocol** | Cam | Giao thức lớp trên (ví dụ: IP, IP6). |
| **Src & Dst IP.Port** | Cam | `IP_Nguồn.Port_Nguồn > IP_Đích.Port_Đích`. (Ví dụ: `172.16.146.2.21`).