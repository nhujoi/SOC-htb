# Phân Tích Các Sự Kiện Độc Hại Với Sysmon & Event Logs

Để đảm bảo an ninh mạng mạnh mẽ, việc xác định và phân tích các sự kiện độc hại là rất quan trọng. Ngoài các ID sự kiện tiêu chuẩn như **4624** (đăng nhập mới) và **4688** (tạo tiến trình mới), việc tích hợp Sysmon giúp mở rộng khả năng bao phủ và phát hiện các hành vi đáng ngờ.

## 1. Cơ Bản Về Sysmon (System Monitor)
Sysmon là một dịch vụ hệ thống và trình điều khiển thiết bị của Windows, hoạt động liên tục qua các lần khởi động lại để giám sát và ghi lại hoạt động hệ thống vào nhật ký sự kiện Windows.



* **Các thành phần chính:**
    * Dịch vụ Windows (Windows service): Giám sát hoạt động hệ thống.
    * Trình điều khiển thiết bị (Device driver): Hỗ trợ thu thập dữ liệu hoạt động.
    * Nhật ký sự kiện (Event log): Hiển thị dữ liệu đã thu thập.
* **Lợi ích:** Ghi lại các thông tin thường không có trong nhật ký Security Event tiêu chuẩn, hỗ trợ đắc lực cho phân tích pháp y.
* **ID Sự kiện (Event IDs):** Phân loại hoạt động theo ID, ví dụ ID 1 là "Process Creation" (Tạo tiến trình) và ID 3 là "Network Connection" (Kết nối mạng).
* **Cấu hình:** Sử dụng file XML để bao gồm hoặc loại trừ các sự kiện dựa trên thuộc tính (tên tiến trình, IP...).
    * Có thể sử dụng các cấu hình phổ biến như của *SwiftOnSecurity* hoặc *Olaf Hartong*.
* **Cài đặt:** `sysmon.exe -i -accepteula -h md5,sha256,imphash -l -n`.
* **Áp dụng cấu hình:** `sysmon.exe -c filename.xml`.

## 2. Ví Dụ Phát Hiện 1: DLL Hijacking (Chiếm đoạt DLL)
Mục tiêu là phát hiện khi một ứng dụng hợp lệ tải một DLL độc hại thay vì DLL chuẩn.



* **Sự kiện trọng tâm:** **Sysmon Event ID 7** (Module load events - Sự kiện tải mô-đun).
* **Cấu hình:** Cần sửa đổi file cấu hình (ví dụ của SwiftOnSecurity) để chuyển từ "exclude" (loại trừ) sang "include" (bao gồm) đối với Event ID 7 nhằm ghi lại dữ liệu cần thiết.
* **Kịch bản tấn công:** Sử dụng `calc.exe` để tải một `WININET.dll` giả mạo (thực chất là một reflective DLL) từ một thư mục có quyền ghi (ví dụ: Desktop).
* **Chỉ số xâm phạm (IOCs) để phát hiện:**
    1.  `calc.exe` chạy từ một thư mục có quyền ghi (lẽ ra phải luôn ở System32 hoặc Syswow64).
    2.  `WININET.dll` được tải từ bên ngoài System32 bởi `calc.exe` (tên DLL này không thể thay đổi nếu kẻ tấn công muốn hijack `calc.exe`).
    3.  DLL được tải không có chữ ký số (unsigned), trong khi bản gốc của Microsoft có chữ ký.

## 3. Ví Dụ Phát Hiện 2: Unmanaged PowerShell/C# Injection
Mục tiêu là phát hiện việc tiêm mã C# (managed code) vào các tiến trình thường không sử dụng .NET (unmanaged processes).



* **Khái niệm:** Mã C# (managed) cần Common Language Runtime (CLR) để thực thi bytecode.
* **Công cụ quan sát:** *Process Hacker* có thể làm nổi bật các tiến trình "managed" (màu xanh lá).
* **Dấu hiệu nhận biết:** Sự xuất hiện của **`clr.dll`** và **`clrjit.dll`** trong danh sách mô-đun của tiến trình.
* **Cơ chế phát hiện:** Nếu `clr.dll` và `clrjit.dll` được tải vào một tiến trình thường không yêu cầu chúng (ví dụ: `spoolsv.exe`), điều này gợi ý một cuộc tấn công "execute-assembly" hoặc tiêm PowerShell.
* **Kiểm chứng:** Sử dụng **Sysmon Event ID 7** để xác thực việc tải các DLL này vào tiến trình mục tiêu.

## 4. Ví Dụ Phát Hiện 3: Credential Dumping (Trích xuất thông tin xác thực)
Mục tiêu là phát hiện các công cụ như Mimikatz cố gắng truy cập LSASS để lấy mật khẩu hoặc mã băm.



* **Công cụ tấn công:** Mimikatz (lệnh `sekurlsa::logonpasswords`) truy cập Local Security Authority Subsystem Service (LSASS).
* **Sự kiện trọng tâm:** **Sysmon Event ID 10** (ProcessAccess - Truy cập tiến trình).
* **Chỉ số xâm phạm (IOCs) để phát hiện:**
    1.  Một file lạ từ thư mục lạ (ví dụ: Downloads) cố gắng truy cập LSASS.
    2.  Người dùng nguồn (SourceUser) khác với người dùng đích (TargetUser) (ví dụ: một user thường truy cập vào tiến trình của SYSTEM).
    3.  Yêu cầu quyền **`SeDebugPrivilege`** (thường dùng để debug).
* *Lưu ý:* Một số tiến trình hợp pháp như Antivirus hoặc EDR cũng có thể truy cập LSASS.