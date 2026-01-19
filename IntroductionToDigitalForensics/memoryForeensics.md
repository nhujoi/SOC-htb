# Memory Forensics (Điều tra số Bộ nhớ)

## 1. Định nghĩa và Quy trình (Definition & Process)

**Định nghĩa:**
Memory forensics (Phân tích bộ nhớ khả biến) là nhánh chuyên biệt của điều tra số, tập trung phân tích RAM của thiết bị. Khác với điều tra ổ cứng truyền thống, nó xử lý trạng thái "sống" (live state) của hệ thống tại một thời điểm cụ thể.

**Dữ liệu có giá trị trong RAM:**
* Kết nối mạng, File handles, File đang mở.
* Registry keys đang mở, Tiến trình (Process) đang chạy.
* Modules và Driver đã tải.
* Lịch sử dòng lệnh (Command history), Session console.
* Cấu trúc dữ liệu Kernel, Thông tin người dùng/xác thực (Credential).
* Dấu vết Malware, Cấu hình hệ thống, Vùng nhớ của tiến trình.
* *Lưu ý:* Mã độc thường để lại dấu vết trong RAM. Đôi khi key giải mã (encryption keys) cũng nằm trong RAM.

**Quy trình 6 bước (Lấy cảm hứng từ SANS):**
1.  **Xác định & Xác minh Tiến trình (Process Identification):** Liệt kê tất cả tiến trình, xác định nguồn gốc, đối chiếu với danh sách sạch, tìm điểm bất thường (tên lạ).
2.  **Đào sâu thành phần Tiến trình (Deep Dive):** Kiểm tra DLLs và Handles của tiến trình nghi ngờ. Tìm kiếm DLL độc hại hoặc dấu hiệu DLL injection/hijacking.
3.  **Phân tích Hoạt động Mạng (Network Activity):** Rất nhiều malware cần kết nối Internet (C2 server). Cần rà soát kết nối active/passive, IP/Domain ngoại lai, mục đích giao tiếp.
4.  **Phát hiện Chèn mã (Code Injection):** Phát hiện kỹ thuật như Process Hollowing hoặc dùng vùng nhớ chưa được ánh xạ (unmapped memory).
5.  **Khám phá Rootkit:** Rootkit ẩn sâu trong OS để duy trì quyền truy cập. Cần quét các thay đổi trong OS, driver có quyền cao bất thường.
6.  **Trích xuất thành phần nghi vấn (Extraction):** Dump (trích xuất) các tiến trình, driver, file thực thi nghi ngờ ra khỏi RAM để phân tích kỹ hơn bằng công cụ chuyên dụng.

---

## 2. Volatility Framework

**Tổng quan:**
Công cụ mã nguồn mở hàng đầu để phân tích bộ nhớ, viết bằng Python. Chạy được trên Windows, Linux, macOS. Hỗ trợ phân tích dump của Windows (XP đến Server 2016), macOS, và Linux.

**Các Modules/Plugins phổ biến:**
* `pslist`: Liệt kê tiến trình đang chạy.
* `cmdline`: Hiển thị tham số dòng lệnh của tiến trình.
* `netscan`: Quét kết nối mạng và cổng mở.
* `malfind`: Quét mã độc tiềm ẩn bị tiêm vào tiến trình.
* `handles`: Quét các handles đang mở (file, registry...).
* `svcscan`: Liệt kê Windows services.
* `dlllist`: Liệt kê DLLs được tải bởi tiến trình.
* `hivelist`: Liệt kê các Registry hives trong bộ nhớ.

---

## 3. Volatility v2 Fundamentals (Cơ bản & Thực hành)

Phần này minh họa việc sử dụng Volatility v2 trên file dump `Win7-2515534d.vmem`.

**A. Xác định Profile (Identifying the Profile):**
* Plugin: `imageinfo`
* Chức năng: Xác định hệ điều hành để Volatility hiểu đúng cấu trúc dữ liệu.
* Ví dụ output: Gợi ý profile `Win7SP1x64`.

**B. Xác định Tiến trình đang chạy (Identifying Running Processes):**
* Plugin: `pslist`
* Cú pháp: `vol.py -f [file] --profile=[profile] pslist`
* Output: Tên tiến trình, PID, PPID (Parent PID), Thời gian bắt đầu (Start Time).

**C. Xác định Dấu vết Mạng (Identifying Network Artifacts):**
* Plugin: `netscan`
* Chức năng: Tìm các kết nối mạng (TCP/UDP), IP Local/Foreign, Trạng thái (LISTENING, ESTABLISHED), PID và Process sở hữu.
* Plugin bổ trợ: `connscan` (Dùng pool tag scanning để tìm cả các kết nối đã chấm dứt - artifacts cũ).

**D. Xác định Mã độc chèn vào (Identifying Injected Code):**
* Plugin: `malfind`
* Chức năng: Tìm và trích xuất code bị tiêm.
* Dấu hiệu: Vùng nhớ có quyền `PAGE_EXECUTE_READWRITE` và header bất thường (ví dụ đầu file là assembly `ADD [EAX], AL` thay vì MZ header chuẩn).

**E. Xác định Handles (Identifying Handles):**
* Plugin: `handles`
* Chức năng: Xem tiến trình đang nắm giữ tài nguyên gì.
* Lọc theo loại:
    * `--object-type=Key`: Xem Registry keys đang mở.
    * `--object-type=File`: Xem File paths đang mở.
    * `--object-type=Process`: Xem các tiến trình con hoặc liên quan.

**F. Xác định Windows Services:**
* Plugin: `svcscan`
* Output: Tên Service, Trạng thái (RUNNING/STOPPED), Binary Path (đường dẫn file thực thi), PID.

**G. Xác định DLLs đã tải (Identifying Loaded DLLs):**
* Plugin: `dlllist`
* Chức năng: Liệt kê các thư viện động (.dll) mà một tiến trình (ví dụ PID 1512 - Ransomware) đang sử dụng. Cung cấp đường dẫn cụ thể của DLL.

**H. Xác định Hives (Registry):**
* Plugin: `hivelist`
* Chức năng: Liệt kê các file Registry (SAM, SYSTEM, SOFTWARE, NTUSER.DAT...) đang nằm trong RAM cùng địa chỉ bộ nhớ ảo/vật lý.

---

## 4. Phân tích Rootkit với Volatility v2

Phần này minh họa trên file dump `rootkit.vmem`.

**Cấu trúc EPROCESS:**
* Là cấu trúc dữ liệu trong Kernel Windows đại diện cho một tiến trình.
* Sử dụng danh sách liên kết đôi (Doubly-linked list) tên là `ActiveProcessLinks` để OS quản lý danh sách tiến trình.
    * `flink` (Forward pointer): Trỏ tới tiến trình kế tiếp.
    * `blink` (Backward pointer): Trỏ tới tiến trình phía trước.

**Kỹ thuật DKOM (Direct Kernel Object Manipulation):**
* Rootkit sử dụng DKOM để "tháo" (unlink) một tiến trình độc hại ra khỏi danh sách `ActiveProcessLinks`.
* Hệ quả: Các công cụ giám sát thông thường (Task Manager) hoặc plugin `pslist` (vốn dựa vào danh sách liên kết này) sẽ không thấy tiến trình đó.

**Phát hiện Rootkit (So sánh pslist vs psscan):**
* `pslist`: Quét dựa trên danh sách liên kết đôi (bị Rootkit qua mặt).
* `psscan`: Quét dựa trên "Pool tag scanning" (tìm các cấu trúc EPROCESS còn tồn tại trong bộ nhớ vật lý mà không cần danh sách liên kết).
* **Ví dụ thực tế:** `pslist` không thấy `test.exe`, nhưng `psscan` tìm thấy `test.exe` (đã bị ẩn). Đây là dấu hiệu của Rootkit.

---

## 5. Phân tích Bộ nhớ sử dụng Strings

Kỹ thuật trích xuất chuỗi ký tự có thể đọc được (human-readable) từ file dump. Công cụ: `Strings` (Sysinternals) hoặc lệnh `strings` (Linux).

**Các trường hợp sử dụng (Use cases):**
1.  **Tìm địa chỉ IPv4:**
    * Dùng Regex (`grep`) để lọc các chuỗi dạng số IP (ví dụ `192.168.182.254`).
2.  **Tìm địa chỉ Email:**
    * Dùng Regex để tìm chuỗi dạng `abc@xyz.com`.
    * Ví dụ tìm thấy: `info@aadobetech.com`, `noreply@vmware.com`.
3.  **Tìm lệnh Command Prompt / PowerShell:**
    * Grep các từ khóa: `cmd`, `powershell`, `bash`.
    * Ví dụ tìm thấy: `cmd.exe /c "C:\Intel\...\tasksche.exe"` (Cho thấy hành vi thực thi mã độc).

-> Regex là công cụ mạnh mẽ để trích xuất dữ liệu thô (raw data extraction) trong quá trình phân tích.