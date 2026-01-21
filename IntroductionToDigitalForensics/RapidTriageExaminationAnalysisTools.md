# Rapid Triage Examination & Analysis Tools

Phân tích Rapid Triage yêu cầu các công cụ chuyên dụng để trích xuất thông tin quan trọng từ thiết bị kỹ thuật số một cách nhanh chóng và chính xác. Bộ công cụ của **Eric Zimmerman** (EZ Tools) là tiêu chuẩn vàng cho nhiệm vụ này.

## 1. Thiết lập và Công cụ
* **Tải xuống:** Sử dụng script PowerShell `Get-ZimmermanTools.ps1` để tự động tải và cập nhật tất cả các công cụ.
* **Các thư mục quan trọng (trong bài Lab):**
    * Evidence: `C:\Users\johndoe\Desktop\forensic_data`
    * KAPE Output: `C:\Users\johndoe\Desktop\forensic_data\kape_output`
    * EZ Tools: `C:\Users\johndoe\Desktop\Get-ZimmermanTools`

## 2. Phân tích Hệ thống Tập tin (Filesystem Analysis)

### A. MAC(b) Times & Timestomping
* **Định nghĩa MAC(b):**
    * **M (Modified):** Thời gian nội dung file bị sửa đổi.
    * **A (Accessed):** Thời gian file được truy cập/đọc.
    * **C (Changed - MFT Record Change):** Thời gian metadata trong MFT thay đổi.
    * **b (Birth):** Thời gian file được tạo ra.
* **Quy tắc NTFS:**
    * *Sao chép (Copy):* File mới kế thừa Modified Time, nhưng cập nhật Accessed và Birth Time (thời điểm copy).
    * *Di chuyển (Move):* Giữ nguyên Timestamp cũ.
* **Timestomping (T1070.006):** Kỹ thuật giả mạo timestamp để ẩn giấu hành vi.
    * **Phát hiện:** Timestamp hiển thị trong Windows Explorer lấy từ thuộc tính `$STANDARD_INFORMATION` (dễ bị sửa). Cần đối chiếu với thuộc tính `$FILE_NAME` (khó sửa hơn, chỉ kernel sửa được).
    * **Công cụ:** `MFTECmd` để so sánh 2 thuộc tính này.

### B. MFT (Master File Table) - $MFT
* **Vai trò:** Cơ sở dữ liệu chứa thông tin về mọi file/thư mục trên phân vùng NTFS (metadata, timestamp, size, permissions).
* **Cấu trúc Record:**
    * *Header:* Metadata của record.
    * *Standard Information:* Timestamp, attributes.
    * *File Name:* Tên file, timestamp phụ.
    * *Data:* Nội dung file (Resident - lưu trong MFT nếu file nhỏ; Non-resident - lưu ngoài cluster nếu file lớn).
* **Công cụ phân tích:**
    * `MFT Explorer`: GUI xem cấu trúc cây thư mục.
    * `MFTECmd`: CLI parser, xuất ra CSV.
    * `Active@ Disk Editor`: Xem dữ liệu thô (Hex).

### C. Zone.Identifier (Mark of the Web - MotW)
* **Khái niệm:** Alternate Data Stream (ADS) được Windows gắn vào file tải từ Internet để xác định vùng bảo mật (ZoneId=3: Internet).
* **Giá trị Forensic:** Chứa `HostUrl` và `ReferrerUrl` -> Xác định nguồn gốc tải file (URL cụ thể).
* **Lệnh kiểm tra:** `Get-Item * -Stream Zone.Identifier` hoặc `Get-Content`.

### D. USN Journal - $J ($Extend\$J)
* **Vai trò:** Ghi lại nhật ký thay đổi của file và thư mục (Tạo, Xóa, Đổi tên, Ghi đè).
* **Phân tích:**
    * Dùng `MFTECmd` để parse `$J` sang CSV.
    * Kết hợp với `Timeline Explorer` để lọc theo Entry ID.
    * Hữu ích để tìm các file đã bị xóa hoặc đổi tên, và các file tải dở (`.crdownload`).

### E. Timeline Explorer
* **Chức năng:** Công cụ GUI để xem và phân tích các file CSV từ các công cụ EZ Tools khác.
* **Tính năng:** Lọc (Filtering), Nhóm (Grouping), Tìm kiếm, Tô màu dữ liệu.

## 3. Phân tích Windows Event Logs

* **Vị trí:** `C:\Windows\System32\winevt\logs` (.evtx).
* **Công cụ: EvtxECmd**
    * Chuyển đổi .evtx sang CSV/JSON/XML.
    * **Maps:** Sử dụng bản đồ để chuẩn hóa dữ liệu (ví dụ: tách User, IP, Command Line ra các cột riêng).
    * Lệnh sync map: `EvtxECmd.exe --sync`.
* **Công cụ: EQL (Event Query Language)**
    * Ngôn ngữ truy vấn để tìm mối đe dọa từ log (đã chuyển sang JSON).
    * Ví dụ phát hiện User Enumeration: `eql query -f data.json "EventId=1 and (Image='*net.exe' ...)"`.

## 4. Phân tích Windows Registry

* **Vị trí:** `C:\Windows\System32\config` (SYSTEM, SOFTWARE, SAM, SECURITY) và User Dir (NTUSER.DAT).
* **Công cụ: Registry Explorer (GUI)**
    * Load nhiều Hive cùng lúc.
    * Hỗ trợ Bookmarks (đánh dấu sẵn các key quan trọng như Run, UserInfo).
    * Khôi phục các key đã xóa.
* **Công cụ: RegRipper (CLI)**
    * Sử dụng Plugins để trích xuất thông tin nhanh.
    * *Plugins phổ biến:* `compname` (tên máy), `timezone`, `nic2` (mạng), `installer`, `recentdocs`, `run` (khởi động cùng win), `bam`.

## 5. Artifacts Thực thi Chương trình (Execution Artifacts)

Đây là các dấu vết chứng minh một chương trình đã từng chạy trên hệ thống.

| Artifact | Vị trí / Mô tả | Công cụ | Thông tin thu được |
| :--- | :--- | :--- | :--- |
| **Prefetch** | `C:\Windows\Prefetch` (.pf) | `PECmd` | Tên App, Đường dẫn, Lần chạy cuối, **Số lần chạy**, Các file/thư mục được tham chiếu. |
| **ShimCache** | Registry (SYSTEM Hive) | `Registry Explorer` | Đường dẫn file, Timestamp (Last Modified), Flag thực thi. |
| **AmCache** | `C:\Windows\AppCompat\Programs\AmCache.hve` | `AmcacheParser` | SHA-1 hash, Đường dẫn, Thời gian cài đặt/chạy đầu tiên. |
| **BAM** | Registry (SYSTEM Hive) | `Registry Explorer` / `RegRipper` | Theo dõi tác vụ nền/lên lịch, bằng chứng thực thi cho từng User. |

## 6. Phân tích API Call (.apmx64) & PowerShell

### API Monitor
* Phân tích file `.apmx64` (ghi lại các lời gọi API).
* **Các hàm API đáng ngờ cần tìm:**
    * `getenv`: Lấy biến môi trường.
    * `RegOpenKey` / `RegSetValueExA`: Thay đổi Registry (thường dùng để tạo Persistence - khởi động cùng Win).
    * `CreateProcessA`: Tạo tiến trình mới.
        * Chú ý tham số `dwCreationFlags`: Nếu là `CREATE_SUSPENDED` -> Dấu hiệu của **Process Injection**.
        * `lpCommandLine`: Lệnh thực thi thực tế.

### PowerShell Activity
* **Nguồn:** PowerShell Transcripts (file log phiên làm việc).
* **Dấu hiệu đáng ngờ:**
    * Lệnh tải file (`Invoke-WebRequest`, `wget`).
    * Lệnh mã hóa/obfuscated (Base64).
    * Thao tác Registry hoặc Scheduled Tasks.
    * Chạy script không được ký (Unsigned scripts).