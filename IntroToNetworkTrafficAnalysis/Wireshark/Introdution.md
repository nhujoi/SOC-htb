# Phân tích với Wireshark và TShark

## 1. Tổng quan về Wireshark
Wireshark là một trình phân tích lưu lượng mạng mã nguồn mở và miễn phí, tương tự như tcpdump nhưng có giao diện đồ họa (GUI). Nó đa nền tảng và có khả năng bắt dữ liệu trực tiếp từ nhiều loại giao diện (WiFi, USB, Bluetooth) và lưu dưới nhiều định dạng. Sức mạnh thực sự của Wireshark nằm ở khả năng phân tích, cung cấp cái nhìn sâu sắc vào các gói tin mạng.

### Các tính năng và khả năng:
* Kiểm tra sâu gói tin (Deep packet inspection) cho hàng trăm giao thức.
* Hỗ trợ cả giao diện đồ họa và dòng lệnh (TTY).
* Chạy trên hầu hết các hệ điều hành.
* Hỗ trợ đa dạng giao thức: Ethernet, IEEE 802.11, PPP/HDLC, ATM, Bluetooth, USB, Token Ring, Frame Relay, FDDI....
* Khả năng giải mã (Decryption) cho: IPsec, ISAKMP, Kerberos, SNMPv3, SSL/TLS, WEP, và WPA/WPA2.



## 2. Yêu cầu sử dụng
### Windows:
* **Universal C Runtime:** Có sẵn trên Win 10/Server 2019, hoặc cần cài KB2999226/KB3118401.
* **Vi xử lý:** 64-bit AMD64/x86-64 hoặc 32-bit x86.
* **RAM:** Tối thiểu 500 MB (file capture lớn cần nhiều hơn).
* **Ổ cứng:** 500 MB trống.
* **Màn hình:** 1280 × 1024 hoặc cao hơn.
* **Card mạng:** Ethernet hoặc 802.11 (cần thiết bị đặc biệt để bắt raw 802.11).
* **Cài đặt:** Tải executable từ wireshark.org.

### Linux:
* Chạy trên hầu hết các nền tảng UNIX/Linux/BSD với cấu hình tương đương Windows.
* Kiểm tra cài đặt: `which wireshark` (thường ở `/usr/sbin/wireshark`).
* Cài đặt: `sudo apt install wireshark`.

## 3. TShark vs. Wireshark (Terminal vs. GUI)
* **TShark:** Công cụ dòng lệnh (terminal) dựa trên Wireshark, chia sẻ cú pháp và tính năng. Phù hợp cho máy không có giao diện desktop hoặc để chuyển dữ liệu sang công cụ khác.
* **Wireshark:** Giao diện GUI đầy đủ tính năng, phù hợp để phân tích trực quan.

### Các cờ (Switches) cơ bản của TShark:
| Cờ | Chức năng |
| :--- | :--- |
| `-D` | Hiển thị các giao diện có sẵn và thoát. |
| `-L` | Liệt kê các loại Link-layer có thể bắt và thoát. |
| `-i` | Chọn giao diện để bắt (vd: `-i eth0`). |
| `-f` | Bộ lọc gói tin theo cú pháp libpcap (dùng khi bắt). |
| `-c` | Bắt một số lượng gói tin cụ thể rồi thoát. |
| `-a` | Điều kiện tự động dừng (thời gian, kích thước file, số gói). |
| `-r` | Đọc từ một file pcap. |
| `-W` | Ghi vào file định dạng pcapng. |
| `-P` | In tóm tắt gói tin trong khi đang ghi vào file (`-W`). |
| `-x` | Thêm đầu ra Hex và ASCII. |
| `-h` | Xem menu trợ giúp (`tshark -h`). |



### Sử dụng TShark cơ bản:
* **Kiểm tra vị trí:** `which tshark`.
* **Liệt kê giao diện:** `tshark -D`.
* **Bắt và ghi file:** `tshark -i 1 -w /tmp/test.pcap`.
* **Đọc file:** `sudo tshark -i eth0 -w /tmp/test.pcap` hoặc dùng `-r` để đọc lại.
* **Áp dụng bộ lọc:** `sudo tshark -i eth0 -f "host 172.16.146.2"`.

## 4. Termshark
Termshark là giao diện người dùng dựa trên văn bản (TUI), cung cấp giao diện giống Wireshark ngay trên terminal.
* Có thể tải từ GitHub hoặc build từ source.
* Khởi động bằng lệnh tương tự TShark/Tcpdump, chỉ định giao diện và bộ lọc.
* Lưu ý: Cửa sổ Termshark chỉ mở khi phát hiện có lưu lượng truy cập khớp với bộ lọc.



## 5. Giao diện Wireshark (GUI Walkthrough)
Giao diện chính gồm 3 khung (Panes):

1.  **Packet List (Danh sách gói tin):**
    * Hiển thị tóm tắt mỗi gói tin.
    * Các cột mặc định: Number (Thứ tự), Time (Thời gian Unix), Source (IP nguồn), Destination (IP đích), Protocol (Giao thức), Information (Thông tin chi tiết tùy giao thức).
2.  **Packet Details (Chi tiết gói tin):**
    * Cho phép kiểm tra sâu các giao thức, phân tách theo mô hình OSI.
    * Hiển thị theo thứ tự đảo ngược: Lớp thấp nhất ở trên, lớp cao nhất ở dưới.
3.  **Packet Bytes (Byte gói tin):**
    * Hiển thị nội dung gói tin dưới dạng ASCII hoặc Hex.
    * Khi chọn một trường ở khung Details, phần byte tương ứng sẽ được tô sáng.



### Các tính năng khác:
* **Toolbar:** Quản lý bắt đầu/dừng, thay đổi giao diện, lưu file (`.pcap`), và áp dụng bộ lọc.
* **Bắt đầu capture:** Chọn giao diện và nhấn Start. Thay đổi tùy chọn sẽ khởi động lại quá trình bắt.
* **Lưu capture:** File -> Save hoặc biểu tượng trên thanh công cụ.

## 6. Xử lý và Lọc (Processing and Filtering)
Có hai loại bộ lọc chính: Capture Filters và Display Filters. Với file lớn, nên chia nhỏ file trước khi xử lý để tránh Wireshark chạy chậm.

### Capture Filters (Bộ lọc khi bắt)
* Được nhập **trước** khi quá trình bắt bắt đầu.
* Sử dụng cú pháp BPF (giống Tcpdump).
* Loại bỏ tất cả lưu lượng không khớp, giúp tiết kiệm dung lượng ổ cứng.
* Cách áp dụng: Menu Capture -> Options -> Nhập vào ô "Capture filter for selected interfaces".

**Các bộ lọc Capture phổ biến:**
| Bộ lọc | Kết quả |
| :--- | :--- |
| `host x.x.x.x` | Chỉ bắt traffic của host cụ thể. |
| `net x.x.x.x/24` | Traffic từ/đến một mạng cụ thể. |
| `src/dst net` | Chỉ định rõ hướng nguồn hoặc đích của mạng. |
| `port #` | Chỉ bắt port cụ thể. |
| `not port #` | Bắt tất cả trừ port đó. |
| `port # and #` | Kết hợp nhiều port. |
| `portrange x-x` | Bắt trong dải port. |
| `ip / ether / tcp` | Chỉ bắt theo protocol header. |
| `broadcast / ...` | Bắt loại traffic cụ thể (unicast/multicast/broadcast). |



### Display Filters (Bộ lọc hiển thị)
* Sử dụng **trong khi** hoặc **sau khi** bắt xong.
* Cú pháp riêng của Wireshark, nhiều tùy chọn hơn.
* Không ghi đè dữ liệu gốc, chỉ ẩn đi những gì không cần xem.
* Cách áp dụng: Nhập vào thanh Filter trên Toolbar (màu xanh lá là đúng cú pháp).

**Các bộ lọc Display phổ biến:**
| Bộ lọc | Kết quả |
| :--- | :--- |
| `ip.addr == x.x.x.x` | Traffic liên quan đến host (OR statement). |
| `ip.src/dst == x` | Traffic từ hoặc đến host cụ thể. |
| `dns / tcp / ftp` | Lọc theo giao thức. |
| `tcp.port == x` | Lọc theo cổng TCP cụ thể. |
| `tcp.port != x` | Loại trừ cổng cụ thể. |
| `and / or / not` | Các toán tử logic. |

**Lưu ý:** Lọc `http` (giao thức) khác với lọc `port 80`. `http` tìm các dấu hiệu của giao thức (GET/POST), còn `port 80` hiển thị mọi thứ đi qua cổng đó bất kể giao thức.