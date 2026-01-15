# Bộ lọc Gói tin trong Tcpdump (Tcpdump Packet Filtering)

Tcpdump cung cấp khả năng phân tích dữ liệu mạnh mẽ thông qua các bộ lọc (filters). Sử dụng bộ lọc giúp giảm thiểu lượng dữ liệu không cần thiết, tiết kiệm dung lượng ổ cứng và tăng tốc độ xử lý.

## 1. Các Bộ lọc và Cú pháp Nâng cao (Advanced Syntax Options)

Bạn có thể lọc theo nhiều tiêu chí: từ IP cụ thể, mạng, cổng, đến các bit cụ thể trong header TCP.

### Bảng các bộ lọc hữu ích (Helpful TCPDump Filters):

| Bộ lọc | Chức năng | Ví dụ |
| :--- | :--- | :--- |
| **host** | Lọc traffic liên quan đến một host cụ thể (IP hoặc tên miền). | `host 172.16.146.2` |
| **src / dest** | Chỉ định hướng đi của gói tin: Nguồn (src) hoặc Đích (dest). | `src host 1.1.1.1` |
| **net** | Lọc traffic theo mạng (subnet CIDR). | `net 192.168.1.0/24` |
| **proto** | Lọc theo loại giao thức (ether, tcp, udp, icmp...). | `udp` hoặc `proto 17` |
| **port** | Lọc theo cổng (2 chiều). | `port 80` |
| **portrange** | Lọc theo một dải cổng. | `portrange 0-1024` |
| **less / greater** | Lọc theo kích thước gói tin (nhỏ hơn / lớn hơn byte). | `less 64` hoặc `greater 500` |
| **and / &&** | Kết hợp 2 điều kiện (Cả hai phải đúng). | `src host X and port Y` |
| **or / \|\|** | Kết hợp 2 điều kiện (Một trong hai đúng là được). | `icmp or port 53` |
| **not / !** | Loại trừ điều kiện. | `not icmp` hoặc `!arp` |

---

## 2. Chi tiết & Ví dụ Minh họa

### Lọc theo Host (Host Filter)
* **Cú pháp:** `host [IP]`
* **Công dụng:** Kiểm tra xem IP đó có xuất hiện trong phần Source hoặc Destination không.
* **Ví dụ:** `sudo tcpdump -i eth0 host 172.16.146.2`
    * Giúp xác định máy chủ đó đang giao tiếp với ai và như thế nào.

### Lọc theo Nguồn/Đích (Source/Destination Filter)
* **Cú pháp:** `src [IP]` hoặc `dst [IP]`
* **Ví dụ:** `sudo tcpdump -i eth0 src host 172.16.146.2`
    * Chỉ bắt các gói tin **được gửi đi từ** `172.16.146.2`.
* **Kết hợp với Port:** `tcp src port 80`
    * Chỉ bắt traffic HTTP (port 80) trả về từ Server (Source port 80). Điều này giúp bạn chỉ nhìn thấy một chiều của cuộc hội thoại (Server -> Client).

### Lọc theo Mạng (Net Filter)
* **Cú pháp:** `net [CIDR]` hoặc kết hợp `dest net`.
* **Ví dụ:** `sudo tcpdump -i eth0 dest net 172.16.146.0/24`
    * Chỉ bắt các gói tin có đích đến nằm trong mạng `172.16.146.0/24`.

### Lọc theo Giao thức (Protocol Filter)
* **Theo tên:** `udp`, `tcp`, `icmp`.
* **Theo số hiệu giao thức:** `proto [number]`.
    * Ví dụ: `proto 17` tương đương với `udp`.
    * Tài liệu tham khảo số hiệu giao thức: [Protocol Numbers](https://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml).

### Lọc theo Cổng (Port Filter)
* **Cú pháp:** `port [number]`
* **Lưu ý:**
    * `port 53`: Bắt cả TCP và UDP port 53 (DNS).
    * `tcp port 80`: Chỉ bắt TCP port 80 (HTTP).

### Lọc theo Dải cổng (Port Range Filter)
* **Cú pháp:** `portrange [start]-[end]`
* **Ví dụ:** `sudo tcpdump -i eth0 portrange 0-1024`
    * Bắt tất cả traffic dùng các cổng đặc quyền (well-known ports). Rất hữu ích để phát hiện hành vi bất thường, ví dụ web server (port 80/443) tự nhiên kết nối ra ngoài bằng cổng lạ > 1024.

### Lọc theo Kích thước (Less/Greater Filter)
* **Cú pháp:** `less [bytes]` hoặc `greater [bytes]`.
* **Ví dụ:** `sudo tcpdump -i eth0 greater 500`
    * Chỉ hiện các gói tin lớn hơn 500 bytes. Giúp lọc bỏ các gói tin điều khiển nhỏ (SYN, ACK, KeepAlive) để tập trung vào các gói tin chứa dữ liệu thực (file transfer).

### Lọc Kết hợp (Logic Operators)
* **AND (`&&`):** `host 192.168.0.1 and port 23` -> Chỉ bắt traffic Telnet của đúng host đó.
* **OR (`||`):** `icmp or host 172.16.146.1` -> Bắt traffic nếu nó là ICMP HOẶC nếu nó liên quan đến host kia.
* **NOT (`!`):** `not icmp` -> Bắt tất cả trừ ICMP. Giúp loại bỏ nhiễu.

---

## 3. Xử lý Trước và Sau khi Bắt (Pre-Capture vs Post-Capture)

* **Pre-Capture (Lọc trực tiếp khi bắt):**
    * Áp dụng filter ngay câu lệnh capture.
    * *Ưu điểm:* File nhỏ gọn.
    * *Nhược điểm:* Dữ liệu bị loại bỏ vĩnh viễn, không thể khôi phục nếu lỡ lọc mất thông tin quan trọng.
* **Post-Capture (Lọc khi đọc file):**
    * Bắt toàn bộ (`-w capture.pcap`), sau đó dùng `tcpdump -r capture.pcap [filter]` để xem.
    * *Ưu điểm:* Giữ lại toàn bộ dữ liệu gốc để điều tra sâu hơn nếu cần.

---

## 4. Mẹo và Thủ thuật (Tips and Tricks)

* **Số thứ tự tuyệt đối (`-S`):** Hiển thị sequence number thực tế (rất dài) thay vì số tương đối. Cần thiết khi đối chiếu log với các công cụ khác.
* **Kết hợp Switches:** `-v` (chi tiết), `-n` (không giải tên), `-c` (số lượng gói), `-A` (hiển thị ASCII).
* **Hiển thị ASCII (`-A`):**
    * Lệnh: `sudo tcpdump -Ar telnet.pcap`
    * Giúp đọc nội dung văn bản rõ (clear text) như trong Telnet hoặc HTTP headers ngay trên màn hình.
* **Chuyển hướng đầu ra (Piping):**
    * Sử dụng `-l` (line buffered) để đẩy dữ liệu ngay lập tức sang công cụ khác như `grep`.
    * **Ví dụ săn tìm Email:** `sudo tcpdump -Ar http.cap -l | grep 'mailto:*'`
    * Giúp trích xuất nhanh các thông tin như email, user-agent, mật khẩu...

### Săn tìm Cờ TCP (TCP Flags Hunting)
Bạn có thể lọc dựa trên các bit cụ thể trong TCP header.
* **Cú pháp:** `tcp[offset] & mask != 0`
* **Ví dụ tìm gói SYN:** `tcpdump -i eth0 'tcp[13] & 2 != 0'`
    * Giải thích: Byte thứ 13 trong TCP header chứa các cờ. Bit thứ 2 (giá trị 2) tương ứng với cờ SYN. Lệnh này kiểm tra xem bit đó có bật (On) không.

sudo tcpdump -n "tcp[tcpflags] & tcp-syn != 0"

### Tài liệu tham khảo RFC (Protocol RFC Links)
Để hiểu sâu cấu trúc gói tin:
* **IP:** RFC 791
* **ICMP:** RFC 792
* **TCP:** RFC 793
* **UDP:** RFC 768