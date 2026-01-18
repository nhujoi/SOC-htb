# Kỹ thuật và Công cụ Thu thập Bằng chứng (Evidence Acquisition Techniques & Tools)

Thu thập bằng chứng là giai đoạn quan trọng trong điều tra số (Digital Forensics), bao gồm việc thu thập các hiện vật kỹ thuật số (digital artifacts) và dữ liệu từ nhiều nguồn khác nhau. Mục tiêu là bảo tồn bằng chứng tiềm năng để phân tích, đảm bảo tính toàn vẹn, xác thực và khả năng chấp nhận trước tòa.

## 1. Sao chép Pháp y (Forensic Imaging)

Forensic Imaging là quá trình tạo ra một bản sao chính xác từng bit (bit-by-bit copy) của phương tiện lưu trữ kỹ thuật số (ổ cứng, SSD, USB, thẻ nhớ...).
* **Mục đích:** Bảo tồn trạng thái gốc của dữ liệu, đảm bảo tính toàn vẹn và giá trị pháp lý. Cho phép điều tra viên phân tích trên bản sao mà không làm hỏng dữ liệu gốc.

### Các công cụ và giải pháp Forensic Imaging:
1.  **FTK Imager:**
    * Phát triển bởi AccessData (nay thuộc Exterro).
    * Công cụ phổ biến nhất, cho phép tạo bản sao hoàn hảo và xem trước nội dung mà không làm thay đổi dữ liệu.
2.  **AFF4 Imager:**
    * Mã nguồn mở, miễn phí.
    * Tính năng: Trích xuất file dựa trên thời gian tạo, phân đoạn volume, và nén ảnh để giảm thời gian sao chép.
3.  **DD và DCFLDD:**
    * Công cụ dòng lệnh trên Unix/Linux/macOS.
    * *DD:* Có sẵn mặc định trên hầu hết các hệ thống Unix.
    * *DCFLDD:* Phiên bản nâng cao của DD dành cho forensics (có thêm tính năng băm/hashing).
4.  **Công cụ Ảo hóa (Virtualization Tools):**
    * Thu thập từ môi trường ảo bằng cách tạm dừng hệ thống và copy thư mục chứa nó.
    * Sử dụng tính năng **Snapshot** có sẵn trong các phần mềm ảo hóa.

### Ví dụ 1: Tạo ảnh đĩa với FTK Imager
Quy trình thực hiện (Yêu cầu thiết bị lưu trữ ngoài để chứa file ảnh):
1.  Chọn **File** -> **Create Disk Image**.
2.  Chọn nguồn (Media Source): Thường là **Physical Drive** hoặc **Logical Drive**.
3.  Chọn ổ đĩa muốn sao chép.
4.  Chọn loại định dạng ảnh đĩa (Image Type).
5.  Nhập thông tin bằng chứng (Evidence details).
6.  Chọn thư mục đích và tên file. Có thể chỉnh cài đặt nén (compression) và phân mảnh (fragmentation).
7.  Nhấn **Start**.
8.  Theo dõi tiến trình. Sau khi xong, xem bản tóm tắt (imaging summary) và xác thực (verification) nếu đã chọn.

### Ví dụ 2: Mount (Gắn) ảnh đĩa với Arsenal Image Mounter
Quy trình mount file `.vmdk` (từ máy ảo VMWare) để phân tích:
1.  Cài đặt và chạy **Arsenal Image Mounter** với quyền **Administrator**.
2.  Chọn **Mount disk image** và trỏ đến file `.vmdk`.
3.  **Quan trọng:** Chọn chế độ **Read-only** (Chỉ đọc) để bảo vệ tính toàn vẹn của bằng chứng gốc.
4.  Sau khi mount, ổ đĩa sẽ xuất hiện trong máy (ví dụ ổ `D:\`).

---

## 2. Trích xuất Bằng chứng Host-based & Phân loại nhanh (Rapid Triage)

### Bằng chứng trên Host (Host-based Evidence)
Hệ điều hành (như Windows) tạo ra vô số artifact từ việc chạy ứng dụng, sửa file, tạo user...



**A. Dữ liệu khả biến (Volatile Data - RAM):**
Dữ liệu biến mất khi tắt máy hoặc logoff. Rất quan trọng để tìm malware vì chúng thường để lại dấu vết trong RAM.
* **Công cụ thu thập RAM:**
    1.  **FTK Imager:** Dùng phổ biến để capture memory.
    2.  **WinPmem:** Driver mã nguồn mở mặc định cho Windows (tách ra từ dự án Rekall).
    3.  **DumpIt:** Tiện ích đơn giản, gộp bộ nhớ vật lý 32-bit và 64-bit vào một file.
    4.  **MemDump:** Công cụ dòng lệnh miễn phí, đơn giản.
    5.  **Belkasoft RAM Capturer:** Miễn phí, mạnh mẽ, có thể vượt qua các cơ chế chống debug/chống dump của malware.
    6.  **Magnet RAM Capture:** Miễn phí, đơn giản.
    7.  **LiME (Linux Memory Extractor):** Module hạt nhân (LKM) cho Linux, hoạt động trong suốt để tránh các biện pháp chống forensics.

* **Ví dụ thu thập RAM:**
    * *Với WinPmem:* Chạy CMD quyền Admin: `winpmem_mini_x64_rc2.exe memdump.raw`
    * *Với Máy ảo (VM):* Suspend máy ảo -> Tìm file `.vmem` trong thư mục máy ảo.

**B. Dữ liệu bất biến (Non-volatile Data - Disk):**
Dữ liệu còn lại sau khi tắt máy: Registry, Windows Event Log, Prefetch, Amcache, IIS logs, Browser history.

### Phân loại nhanh (Rapid Triage) & KAPE
Mục tiêu: Tập trung thu thập dữ liệu giá trị cao để phân tích nhanh, thay vì sao chép toàn bộ ổ cứng.

**KAPE (Kroll Artifact Parser and Extractor):**
Công cụ mạnh mẽ để thu thập và xử lý artifact từ Windows.
* **Nguyên lý:**
    * **Targets (Mục tiêu):** Định nghĩa *cái gì* cần lấy (file artifacts). File cấu hình đuôi `.tkape`.
        * Ví dụ: `RegistryHivesSystem.tkape` sẽ lấy các file SAM, SYSTEM... từ `C:\Windows\System32\config`.
    * **Modules:** Định nghĩa *làm gì* với dữ liệu đã lấy (xử lý, phân tích).
    * **Compound Targets:** Kết hợp nhiều Targets lại với nhau để chạy 1 lần (Ví dụ: `!SANS_Triage`).
* **Chế độ:** Có cả CLI (`kape.exe`) và GUI (`gkape.exe`).



**Quy trình chạy KAPE (GUI):**
1.  Chọn **Target source** (Ổ đĩa cần lấy, ví dụ `D:\`).
2.  Chọn **Target destination** (Nơi lưu dữ liệu thô thu được).
3.  Chọn **Targets** (Ví dụ `!SANS_Triage`).
4.  Nhấn **Execute**.
5.  KAPE sẽ copy các file (kể cả file đang bị khóa bởi hệ điều hành như `$MFT`, `$LogFile` nhờ cơ chế riêng) sang thư mục đích.

### Thu thập từ xa (Remote Collection) với EDR & Velociraptor
* **EDR (Endpoint Detection and Response):** Cho phép thu thập bằng chứng từ xa trên toàn mạng lưới thay vì từng máy lẻ.
* **Velociraptor:**
    * Sử dụng ngôn ngữ truy vấn VQL (Velociraptor Query Language).
    * Thực hiện các cuộc **Hunts** để thu thập artifact hàng loạt.
    * Tận dụng các định nghĩa của KAPE thông qua **Windows.KapeFiles.Targets**.
    * **Quy trình:** Tạo Hunt mới -> Chọn Artifact (`Windows.KapeFiles.Targets` hoặc `Windows.Memory.Acquisition` cho RAM) -> Launch -> Download kết quả.

---

## 3. Trích xuất Bằng chứng Mạng (Network Evidence)

Dữ liệu mạng là nền tảng cho bất kỳ SOC Analyst nào.

1.  **Phân tích Gói tin (Traffic Capture Analysis):**
    * Công cụ: **Wireshark**, **tcpdump**.
    * Chức năng: Chụp ảnh (snapshot) mọi cuộc hội thoại kỹ thuật số, cho phép xem chi tiết từng gói tin (granular view).
2.  **IDS/IPS:**
    * **IDS (Intrusion Detection System):** Giám sát và cảnh báo khi thấy hoạt động đáng ngờ.
    * **IPS (Intrusion Prevention System):** Phát hiện và tự động chặn hành động độc hại.
3.  **Traffic Flow (Luồng dữ liệu):**
    * Công cụ: **NetFlow**, **sFlow**.
    * Chức năng: Cung cấp cái nhìn tổng quan (high-level) về mẫu lưu lượng mạng (ai nói chuyện với ai, bao lâu), không đi sâu vào nội dung gói tin.
4.  **Firewalls (Tường lửa):**
    * Chức năng: Chặn/cho phép traffic dựa trên luật.
    * Firewall hiện đại có thể nhận diện ứng dụng, user và chặn mối đe dọa.
    * **Log Firewall:** Dùng để phát hiện nỗ lực khai thác lỗ hổng hoặc truy cập trái phép.