# Kỹ thuật & Công cụ Thu thập Bằng chứng (Evidence Acquisition Techniques & Tools)

Thu thập bằng chứng là giai đoạn then chốt trong điều tra số, bao gồm việc thu thập các hiện vật kỹ thuật số và dữ liệu từ nhiều nguồn khác nhau để bảo quản cho quá trình phân tích. Quá trình này yêu cầu các công cụ và kỹ thuật chuyên biệt để đảm bảo tính toàn vẹn (integrity), tính xác thực (authenticity) và khả năng được chấp nhận trước tòa (admissibility).



## 1. Sao chụp Pháp y (Forensic Imaging)

Đây là quy trình cơ bản liên quan đến việc tạo ra một bản sao chính xác từng bit (bit-by-bit copy) của phương tiện lưu trữ kỹ thuật số (HDD, SSD, USB...).

* **Mục đích:** Bảo tồn trạng thái gốc của dữ liệu, đảm bảo tính toàn vẹn và không làm thay đổi bằng chứng gốc trong quá trình phân tích.

### Các công cụ & giải pháp phổ biến:

| Công cụ | Mô tả |
| :--- | :--- |
| **FTK Imager** | Công cụ phổ biến nhất. Tạo bản sao đĩa hoàn hảo, cho phép xem và phân tích nội dung mà không làm thay đổi dữ liệu. |
| **AFF4 Imager** | Mã nguồn mở, miễn phí. Nổi bật với khả năng nén, trích xuất tệp dựa trên thời gian tạo và hỗ trợ nhiều hệ thống tệp. |
| **DD và DCFLDD** | Tiện ích dòng lệnh trên Unix/Linux/macOS. `dd` là công cụ cơ bản, `dcfldd` là bản nâng cao hỗ trợ hashing (băm) để kiểm tra toàn vẹn. |
| **Virtualization Tools** | Đối với máy ảo (VM), bằng chứng được thu thập bằng cách tạm dừng hệ thống hoặc sử dụng tính năng Snapshot (chụp nhanh). |



### Quy trình mẫu:
1.  **Tạo ảnh đĩa với FTK Imager:** Chọn nguồn (Physical/Logical Drive) -> Chọn đích đến -> Chọn loại ảnh (E01, Raw...) -> Bắt đầu (Start).
2.  **Mount ảnh đĩa với Arsenal Image Mounter:** Cho phép gắn (mount) các file ảnh đĩa (như `.vmdk`, `.e01`) thành một ổ đĩa thực trong Windows.
    * *Lưu ý quan trọng:* Luôn chọn chế độ **Read-only** (Chỉ đọc) để bảo vệ tính toàn vẹn của bằng chứng gốc.

---

## 2. Trích xuất Bằng chứng trên Máy chủ & Phân loại Nhanh (Host-based Evidence & Rapid Triage)

### 2.1. Host-based Evidence (Bằng chứng trên máy chủ)
Hệ điều hành hiện đại tạo ra vô số dấu vết từ việc chạy ứng dụng, sửa file hay tạo tài khoản người dùng.

* **Dữ liệu dễ bay hơi (Volatile Data):** Dữ liệu biến mất khi tắt máy hoặc đăng xuất (ví dụ: RAM).
    * *Tầm quan trọng:* Rất quan trọng khi điều tra mã độc (malware) vì mã độc thường để lại dấu vết trong RAM.
* **Dữ liệu không bay hơi (Non-volatile Data):** Tồn tại trên ổ cứng (Registry, Event Logs, Prefetch...).

**Các công cụ thu thập bộ nhớ (RAM):**
* **WinPmem:** Driver thu thập bộ nhớ mã nguồn mở mặc định cho Windows.
* **DumpIt:** Tiện ích đơn giản, gộp bộ nhớ hệ thống 32-bit và 64-bit vào một file output duy nhất.
* **MemDump:** Công cụ dòng lệnh nhẹ, bắt nội dung RAM nhanh chóng.
* **Belkasoft RAM Capturer:** Công cụ miễn phí mạnh mẽ, có khả năng vượt qua các cơ chế chống debug/dumping của mã độc.
* **Magnet RAM Capture:** Công cụ miễn phí, đơn giản từ Magnet Forensics.
* **LiME (Linux Memory Extractor):** Module hạt nhân (LKM) cho Linux, hoạt động trong suốt với hệ thống đích để tránh các biện pháp chống điều tra.



### 2.2. Rapid Triage (Phân loại nhanh)
Thay vì sao chụp toàn bộ ổ cứng (tốn thời gian), phương pháp này tập trung thu thập dữ liệu có giá trị cao nhất để phân tích nhanh.

**Công cụ tiêu biểu: KAPE (Kroll Artifact Parser and Extractor)**
KAPE giúp thu thập và xử lý các hiện vật pháp y từ Windows một cách nhanh chóng.
* **Cơ chế hoạt động:**
    * **Targets (Mục tiêu):** Định nghĩa *cái gì* cần lấy (ví dụ: Registry, Event Logs). File cấu hình đuôi `.tkape`.
    * **Modules:** Định nghĩa *làm gì* với dữ liệu đã lấy (ví dụ: Phân tích cú pháp file prefetch).
* **Chế độ:** Hỗ trợ cả dòng lệnh (CLI - `kape.exe`) và giao diện đồ họa (GUI - `gkape.exe`).
* **Compound Targets:** Cho phép gom nhiều Targets lại để chạy một lần (ví dụ: `!SANS_Triage`).



**Thu thập từ xa với Velociraptor:**
* Dùng cho quy mô lớn (EDR).
* Sử dụng truy vấn VQL (Velociraptor Query Language) để săn tìm (Hunt) các hiện vật trên toàn mạng.
* Có thể tích hợp logic thu thập của KAPE thông qua `Windows.KapeFiles.Targets`.
* Hỗ trợ dump RAM từ xa qua `Windows.Memory.Acquisition`.

---

## 3. Trích xuất Bằng chứng Mạng (Extracting Network Evidence)

Bằng chứng mạng cung cấp cái nhìn về luồng dữ liệu và giao tiếp trong hệ thống.

* **Traffic Capture (Thu thập lưu lượng):**
    * Sử dụng **Wireshark** hoặc **tcpdump**.
    * Là "bản chụp" chi tiết từng gói tin (packet) đang di chuyển.
* **IDS/IPS Data:**
    * Hệ thống phát hiện/ngăn chặn xâm nhập cung cấp cảnh báo về các hoạt động đáng ngờ hoặc độc hại đã được nhận diện.
* **Traffic Flow (Luồng lưu lượng):**
    * Sử dụng **NetFlow** hoặc **sFlow**.
    * Cung cấp cái nhìn tổng quan: Ai nói chuyện với ai, khi nào, bao nhiêu dữ liệu (metadata của kết nối), nhưng không chứa nội dung gói tin.
* **Firewall Logs (Nhật ký tường lửa):**
    * Ghi lại các kết nối được phép (allow) hoặc bị chặn (block).
    * Giúp xác định nỗ lực khai thác lỗ hổng hoặc truy cập trái phép.