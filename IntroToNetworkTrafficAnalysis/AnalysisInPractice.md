# Phân tích Lưu lượng Mạng trong Thực tế (Analysis in Practice)

Phân tích lưu lượng mạng (NTA) không phải là một quy trình cứng nhắc hay một vòng lặp trực tiếp. Nó là một quá trình động, chịu ảnh hưởng bởi mục tiêu (tìm lỗi mạng hay tìm hành vi độc hại) và khả năng quan sát (visibility) của bạn. Quy trình phân tích được chia thành 4 trụ cột chính:




## 1. Phân tích Mô tả (Descriptive Analysis)
**Mục tiêu:** Mô tả tập dữ liệu dựa trên các đặc điểm riêng lẻ, giúp phát hiện lỗi thu thập hoặc các giá trị ngoại lai. Đây là bước xác định "Cái gì đang xảy ra?".

* **Xác định vấn đề:** Là vi phạm bảo mật hay lỗi mạng?
* **Xác định phạm vi & mục tiêu:**
    * *Cái gì:* Ví dụ, nhiều máy chủ tải file độc hại từ `bad.example.com`.
    * *Thời gian:* Trong 48 giờ qua + 2 giờ tới.
    * *Thông tin hỗ trợ:* Tên file (`superbad.exe`, `new-crypto-miner.exe`).
* **Xác định đối tượng (Target):**
    * Mạng lưới: `192.168.100.0/24`.
    * Giao thức: HTTP và FTP.

## 2. Phân tích Chẩn đoán (Diagnostic Analysis)
**Mục tiêu:** Làm rõ nguyên nhân, kết quả và sự tương tác. Tìm kiếm lý do cho các sự kiện (nhìn lại quá khứ). Đây là bước xác định "Tại sao nó xảy ra?".

* **Thu thập lưu lượng:**
    * Cắm thiết bị vào liên kết truy cập mạng mục tiêu để bắt traffic trực tiếp (live).
    * Yêu cầu Admin trích xuất file PCAP hoặc dữ liệu Netflow từ SIEM cho dữ liệu lịch sử.
* **Nhận diện & Lọc thành phần cần thiết:**
    * Loại bỏ các gói tin nhiễu (traffic bình thường theo baseline).
    * Giữ lại traffic liên quan đến phạm vi điều tra (HTTP/FTP từ subnet mục tiêu, các yêu cầu GET chứa file nghi ngờ).
* **Hiểu về lưu lượng đã bắt:**
    * Đào sâu vào dữ liệu còn lại.
    * *Ví dụ:* Lọc `ftp-data` để tìm file đã chuyển và tái tạo chúng. Lọc `http.request.method == "GET"` để tìm ai đã tải file.
    * Mục đích: Xác thực nguyên nhân gốc rễ.



## 3. Phân tích Dự báo (Predictive Analysis)
**Mục tiêu:** Đánh giá dữ liệu lịch sử và hiện tại để tạo mô hình dự đoán xác suất tương lai. Xác định xu hướng và phát hiện sai lệch sớm.

* **Ghi chú & Sơ đồ tư duy (Mind mapping):**
    * Ghi lại mọi thứ: Khung thời gian bắt gói tin, các host đáng ngờ, các cuộc hội thoại chứa file độc hại (kèm timestamp, số thứ tự gói).
* **So sánh & Đánh giá:**
    * So sánh dữ liệu tìm được với mức chuẩn (Baseline) và các dấu hiệu tấn công đã biết (signatures).
* **Tổng hợp:**
    * Viết tóm tắt rõ ràng, ngắn gọn về những gì tìm thấy.
    * Mục đích: Để cấp trên đưa ra quyết định (cách ly host, kích hoạt quy trình Ứng phó sự cố - IR).

## 4. Phân tích Đề xuất (Prescriptive Analysis)
**Mục tiêu:** Đưa ra hành động cụ thể để loại bỏ vấn đề hoặc ngăn chặn tái diễn. Đây là đỉnh cao của quy trình.

* **Ra quyết định:** Dựa trên kết quả phân tích để đưa ra giải pháp xử lý triệt để.
* **Bài học kinh nghiệm (Lessons Learned):**
    * Sau khi giải quyết xong, cần nhìn lại toàn bộ quá trình.
    * Tài liệu hóa: Cái gì làm đúng? Cái gì thất bại? Cái gì cần cải thiện?

---

## Mẫu Quy trình (Workflow Template)
Quy trình này thường có tính chu kỳ (cyclic), có thể cần chạy lại các bước để xây dựng bức tranh lớn hơn.

1.  **Vấn đề là gì?** (Nghi ngờ rò rỉ dữ liệu hay lỗi mạng?).
2.  **Định nghĩa phạm vi & mục tiêu** (Mục tiêu, thời gian, tên file/loại file).
3.  **Định nghĩa đối tượng** (Mạng, host, giao thức).
4.  **Thu thập traffic** (Live capture hoặc Historical data).
5.  **Lọc dữ liệu** (Loại bỏ nhiễu, giữ lại traffic liên quan HTTP/FTP/GET request).
6.  **Phân tích & Hiểu** (Tái tạo file, tìm nguồn gốc tải file).
7.  **Ghi chú & Tổng hợp** (Ghi chép chi tiết timestamp, host, kết quả phân tích để báo cáo).

---

## Các thành phần chính của Phân tích hiệu quả

### 1. Hiểu môi trường của bạn (Know your environment)
* Để biết một host là "giả mạo" (rogue) hay không, bạn cần biết những gì được phép tồn tại.
* **Yêu cầu:** Duy trì kiểm kê tài sản (asset inventories) và bản đồ mạng (network maps).

### 2. Vị trí là chìa khóa (Placement is Key)
* Đặt công cụ thu thập ở nơi **gần nguồn vấn đề nhất**.
* *Tấn công từ Internet:* Lắng nghe tại các liên kết inbound (đầu vào).
* *Vấn đề nội bộ:* Đặt công cụ trong cùng phân đoạn mạng (segment) với host bị lỗi.

### 3. Sự kiên trì (Persistence)
* Các vấn đề không phải lúc nào cũng dễ thấy hoặc diễn ra thường xuyên.
* *Ví dụ:* Máy chủ C&C (Command & Control) của hacker có thể chỉ liên lạc 1 lần/ngày. Nếu bỏ lỡ lần đầu, phải đợi rất lâu mới thấy lại.
* Không được nản chí, sự kiên trì quyết định việc ngăn chặn thành công hay bị mã hóa dữ liệu (ransomware).

---

## Phương pháp Tiếp cận Phân tích (Analysis Approach)
Những "chiến thắng dễ dàng" (easy wins) khi bắt đầu tìm kiếm vấn đề:

1.  **Bắt đầu với Giao thức chuẩn (Standard Protocols):**
    * Hầu hết tấn công đến từ Internet, nên hãy kiểm tra HTTP/S, FTP, Email, TCP/UDP cơ bản trước.
    * Sau đó kiểm tra SSH, RDP, Telnet. (Lưu ý: Chính sách bảo mật có cho phép RDP từ ngoài vào không?).
2.  **Tìm kiếm Mẫu hình (Patterns):**
    * Một host kết nối đến Internet vào cùng một giờ mỗi ngày? -> Dấu hiệu điển hình của C&C (Beaconing).
3.  **Kiểm tra Host-to-Host:**
    * Trong mạng chuẩn, các máy trạm (User PCs) **hiếm khi** nói chuyện trực tiếp với nhau.
    * Thông thường chúng chỉ nói chuyện với Cơ sở hạ tầng (DNS, DHCP, DC) hoặc Internet. Hãy nghi ngờ bất kỳ kết nối ngang hàng nào.
4.  **Tìm kiếm sự kiện Độc nhất (Unique events):**
    * Một host thay đổi thói quen truy cập web đột ngột.
    * User-Agent string lạ không khớp với ứng dụng doanh nghiệp.
    * Một cổng ngẫu nhiên chỉ mở 1-2 lần (có thể là C2 callback hoặc ứng dụng lạ).
5.  **Đừng ngại nhờ trợ giúp:**
    * Nhìn quá lâu vào packet captures có thể bị "mù quáng". Một cặp mắt thứ hai có thể phát hiện những gì bạn bỏ qua.

**Tóm lại:** Phân tích là kỹ năng động. Đừng chỉ dựa vào Wireshark/Tcpdump mà hãy kết hợp với Snort, Security Onion, Firewalls, và SIEM để làm giàu thông tin.