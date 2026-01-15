# Quy trình Phân tích Lưu lượng Mạng (The Analysis Process)

Phân tích lưu lượng mạng (NTA) là một quy trình động, thay đổi tùy thuộc vào công cụ, quyền hạn và khả năng quan sát (visibility) mạng. Mục tiêu là xây dựng một quy trình lặp lại được để kiểm tra, phát hiện và ngăn chặn các sự cố.



## 1. Bản chất và Mục tiêu của NTA
* **Định nghĩa:** Là việc kiểm tra chi tiết một sự kiện hoặc quy trình để xác định nguồn gốc và tác động của nó.
* **Mục tiêu chính:**
    * Phân nhỏ dữ liệu thành các phần dễ hiểu.
    * Phát hiện các sai lệch so với lưu lượng thông thường (Baseline).
    * Nhận diện lưu lượng độc hại (ví dụ: truy cập trái phép qua RDP, SSH, Telnet từ internet).
    * Nhận diện các dấu hiệu tiền sự cố mạng.
* **Kết hợp:** Quan sát xu hướng lưu lượng và so sánh với mức chuẩn (baseline) vận hành.

## 2. Lợi ích và Ứng dụng
* **Khả năng quan sát (Visibility):** Đây là lợi ích lớn nhất, giúp quản trị viên nắm được mức sử dụng mạng, các máy chủ giao tiếp nhiều nhất (top-talking hosts) và luồng giao tiếp nội bộ.
* **Công cụ phòng thủ:** NTA kết hợp với IDS/IPS, tường lửa, và các hệ thống SIEM (Splunk, ELK Stack) để cảnh báo nhanh về hành động độc hại.
* **Vận hành hàng ngày:** Giúp khắc phục sự cố kết nối (troubleshooting) và kiểm tra xem cơ sở hạ tầng có hoạt động đúng thiết kế không.
* **Yếu tố con người:** Không nên phụ thuộc hoàn toàn vào công cụ tự động. Cần kết hợp kỹ năng phân tích thủ công (manual checks) của con người để phát hiện những mối đe dọa tinh vi mà công cụ có thể bỏ qua.

---

## 3. Các phương pháp Thu thập và Phụ thuộc (Analysis Dependencies)

Có hai phương pháp chính để thu thập và phân tích: **Thụ động (Passive)** và **Chủ động (Active)**.



### Bảng so sánh các yếu tố phụ thuộc:

| Yếu tố phụ thuộc | Passive (Thụ động) | Active (Chủ động) | Mô tả chi tiết |
| :--- | :---: | :---: | :--- |
| **Quyền hạn (Permission)** | ☑ | ☑ | Bắt buộc phải có sự cho phép bằng văn bản từ người có thẩm quyền để đảm bảo tính pháp lý và đạo đức, đặc biệt trong các môi trường nhạy cảm (ngân hàng, y tế). |
| **Cổng phản chiếu (Mirrored Port/SPAN)** | ☑ | ☐ | Cấu hình Switch/Router để sao chép dữ liệu sang một cổng cụ thể. Yêu cầu NIC ở chế độ *promiscuous*. Hạn chế: Chỉ bắt được traffic trong broadcast domain hoặc phải kết nối đúng SSID (với Wifi). |
| **Công cụ thu thập (Capture Tool)** | ☑ | ☑ | Máy tính cài đặt TCPDump, Wireshark, Netminer... Lưu ý: File PCAP rất lớn và việc lọc (filter) tốn nhiều tài nguyên xử lý. |
| **Đặt thiết bị nội tuyến (In-line Placement)** | ☐ | ☑ | Yêu cầu thay đổi cấu trúc mạng (Topology). Thiết bị đóng vai trò như một "next hop" vô hình. |
| **Network Tap / Multi-NIC** | ☐ | ☑ | Sử dụng thiết bị TAP hoặc máy tính có 2 NIC để dòng dữ liệu chảy qua. Tốt nhất nên đặt ở liên kết Layer 3 giữa các phân đoạn mạng để bắt traffic định tuyến ra ngoài. |
| **Lưu trữ & Sức mạnh xử lý** | ☑ | ☑ | **Passive:** Giống như hứng nước từ vòi phun (ổn định, dễ quản lý).<br>**Active:** Giống như dùng vòi cứu hỏa đổ vào tách trà (áp lực lớn, dữ liệu nhiều, yêu cầu phần cứng cực mạnh). |

---

## 4. Tầm quan trọng của Mức chuẩn (Baseline)

Mặc dù không phải là yêu cầu bắt buộc về mặt kỹ thuật, nhưng việc hiểu rõ **lưu lượng hàng ngày (Baseline)** là yếu tố then chốt để phân tích thành công.



### Tại sao cần Baseline?
* **Lọc nhiễu:** Giúp loại bỏ nhanh chóng các giao tiếp "tốt" đã biết để tập trung vào sự bất thường.
* **Tăng tốc độ:** Giúp phát hiện vấn đề nhanh hơn thay vì phải mò mẫm trong biển dữ liệu.

### Ví dụ thực tế (Scenario):
* **Tình huống:** Mạng công ty bị chậm (latency cao) và xuất hiện file lạ.
* **Không có Baseline:** Bạn bắt được hàng tấn dữ liệu. Bạn phải kiểm tra từng kết nối xem nó có hợp lệ không, máy nào là máy lạ... Quá trình này cực kỳ tốn thời gian và khó khăn.
* **Có Baseline:**
    1.  Dùng công cụ (như Wireshark) để xem "Top talkers".
    2.  Loại bỏ các giao tiếp bình thường.
    3.  **Phát hiện bất thường:** Thấy 2 máy tính người dùng (User PCs) nói chuyện với nhau qua cổng **8080** hoặc **445** (SMB).
    4.  **Kết luận:** Thông thường Web/SMB chỉ đi từ Client đến Server. Việc 2 Client kết nối trực tiếp qua các cổng này là dấu hiệu cực kỳ khả nghi (có thể là lây lan ngang hàng của mã độc hoặc kẻ tấn công).

**Kết luận:** Việc hiểu rõ luồng giao thông mạng và hành vi chuẩn của các giao thức giúp giảm thiểu thiệt hại và tăng tốc độ phản ứng khi có sự cố xâm nhập.