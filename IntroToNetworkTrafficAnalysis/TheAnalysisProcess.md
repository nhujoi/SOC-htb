# Quy trình Phân tích & Kỹ thuật Thu thập (The Analysis Process & Dependencies)

Phân tích lưu lượng mạng (NTA) là một quy trình động, mục tiêu là chia nhỏ dữ liệu để tìm ra các sai lệch so với mức chuẩn (baseline), phát hiện mã độc hoặc các truy cập trái phép (như RDP, SSH, Telnet từ internet).

## 1. Hai Phương Pháp Thu Thập & Phân Tích (Analysis Dependencies)

Việc thu thập và phân tích lưu lượng có thể được thực hiện theo **2 phương pháp riêng biệt**: **Thụ động (Passive)** và **Chủ động (Active)**. Dưới đây là chi tiết từng phương pháp và các yếu tố phụ thuộc:

### Phương pháp 1: Thụ động (Passive Analysis)
Đây là phương pháp "sao chép" dữ liệu để quan sát mà không tương tác trực tiếp hay làm ảnh hưởng đến luồng gói tin gốc.

* **Cơ chế:** Chỉ sao chép (copy) dữ liệu đi qua để phân tích.
* **Yêu cầu kỹ thuật (Dependencies):**
    1.  **Cổng phản chiếu (Mirrored Port / SPAN):**
        * Cần cấu hình Switch/Router để sao chép dữ liệu từ các nguồn khác đến cổng của bạn.
        * Card mạng (NIC) phải đặt ở chế độ **Promiscuous mode** (chế độ hỗn tạp) để nhận toàn bộ gói tin.
        * *Hạn chế:* Chỉ bắt được traffic trong cùng miền quảng bá (Broadcast domain). Với Wifi, phải kết nối vào đúng SSID mới bắt được dữ liệu (nếu chỉ nghe thụ động bên ngoài chỉ thấy các gói tin quảng bá SSID).
    2.  **Lưu trữ & Xử lý:**
        * Lượng dữ liệu giống như "hứng nước từ vòi phun" (ổn định, dễ quản lý hơn Active).

### Phương pháp 2: Chủ động (Active / In-line Analysis)
Đây là phương pháp can thiệp trực tiếp vào đường dây, dữ liệu sẽ chảy *xuyên qua* thiết bị thu thập.

* **Cơ chế:** Thiết bị thu thập nằm chắn giữa đường truyền (In-line).
* **Yêu cầu kỹ thuật (Dependencies):**
    1.  **Thay đổi cấu trúc mạng (In-line Placement):**
        * Yêu cầu thay đổi sơ đồ đấu nối (topology).
        * Thiết bị thu thập đóng vai trò như một "next hop" vô hình về mặt định tuyến/chuyển mạch.
    2.  **Thiết bị phần cứng (Network Tap / Multi-NIC):**
        * Cần máy tính có 2 Card mạng (NIC) hoặc thiết bị TAP chuyên dụng.
        * Nó hoạt động như việc thêm một Router vào giữa liên kết.
        * *Vị trí tối ưu:* Đặt tại liên kết Layer 3 giữa các phân đoạn mạng để bắt traffic định tuyến ra ngoài (không bị giới hạn bởi VLAN như phương pháp Passive).
    3.  **Lưu trữ & Xử lý (Cực lớn):**
        * Lượng dữ liệu ở liên kết Layer 3 lớn hơn nhiều so với trong mạng LAN.
        * *Ví dụ so sánh:* Giống như dùng "vòi rồng cứu hỏa" để rót nước vào tách trà. Áp lực rất lớn, đòi hỏi phần cứng cực mạnh để xử lý và ổ cứng lớn để lưu trữ.

---

## 2. Các yếu tố chung cho cả hai phương pháp

Dù chọn phương pháp nào, bạn cũng phải tuân thủ các yếu tố sau:

* **Giấy phép (Permission - Quan trọng nhất):**
    * Bắt buộc phải có văn bản đồng ý từ người có thẩm quyền.
    * Việc thu thập dữ liệu trái phép có thể vi phạm pháp luật (đặc biệt trong ngân hàng, y tế).
* **Công cụ thu thập (Capture Tool):**
    * Cần máy tính cài các phần mềm như TCPDump, Wireshark, Netminer.
    * *Lưu ý:* File PCAP rất lớn. Mỗi khi lọc (filter) trên Wireshark, máy phải phân tích lại toàn bộ file, rất tốn RAM và CPU.

---

## 3. Tầm quan trọng của Mức chuẩn (Baseline) trong Phân tích

Đây là kỹ năng phân tích cốt lõi để biết "cái gì là bình thường" và "cái gì là bất thường".

### Tại sao cần Baseline?
Nếu không biết lưu lượng mạng hàng ngày trông như thế nào, việc phân tích sẽ cực kỳ tốn thời gian vì phải kiểm tra từng kết nối xem nó có hợp lệ không.

### Ví dụ thực tế (Scenario trong tài liệu):
* **Tình huống:** Mạng bị chậm và xuất hiện file lạ. Bạn bắt gói tin để kiểm tra.
* **Phân tích dựa trên Baseline:**
    1.  Dùng công cụ lọc ra các máy gửi nhiều dữ liệu nhất (Top talkers).
    2.  Loại bỏ các luồng giao tiếp đã biết là an toàn (Known-good).
    3.  **Phát hiện bất thường:** Bạn thấy 2 máy tính người dùng (User PCs) kết nối với nhau qua cổng **8080** (Web) và **445** (SMB).
    4.  **Kết luận:**
        * Thông thường, Web và SMB chỉ đi theo chiều **Client (Máy dùng) -> Server (Máy chủ)**.
        * Việc 2 máy Client nói chuyện trực tiếp với nhau qua các cổng này là **Rất Khả Nghi**. Đây là dấu hiệu của việc lây lan mã độc ngang hàng (lateral movement) hoặc kẻ tấn công đang khai thác tài khoản người dùng.