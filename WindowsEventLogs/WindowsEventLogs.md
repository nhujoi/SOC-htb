# Nhật Ký Sự Kiện Windows (Windows Event Logs)

## 1. Cơ Bản Về Nhật Ký Sự Kiện Windows
Nhật ký sự kiện Windows (Windows Event Logs) là thành phần cốt lõi của Hệ điều hành Windows, có chức năng lưu trữ nhật ký từ hệ thống, ứng dụng, dịch vụ, và các nhà cung cấp ETW (Event Tracing for Windows).
* **Mục đích:** Cung cấp khả năng ghi nhật ký toàn diện cho lỗi ứng dụng, sự kiện bảo mật và thông tin chẩn đoán, hỗ trợ các chuyên gia an ninh mạng phân tích và phát hiện xâm nhập.
* **Phân loại:** Các sự kiện được phân loại vào các nhật ký khác nhau như "Application", "System", "Security" để dễ quản lý theo nguồn hoặc mục đích.
* **Truy cập:** Có thể xem qua ứng dụng **Event Viewer** hoặc lập trình qua API như Windows Event Log API. Quyền quản trị viên cho phép khám phá nhiều nhật ký hơn.
* **Các loại nhật ký mặc định:**
    * **Application:** Lỗi ứng dụng.
    * **Security:** Sự kiện bảo mật.
    * **Setup:** Hoạt động cài đặt hệ thống.
    * **System:** Thông tin chung về hệ thống.
    * **Forwarded Events:** Chứa dữ liệu nhật ký được chuyển tiếp từ các máy khác, giúp quản trị viên có cái nhìn tổng hợp.
* **Lưu trữ:** Event Viewer có thể mở và hiển thị các tệp `.evtx` đã lưu trước đó trong phần "Saved Logs".

## 2. Cấu Trúc Của Một Nhật Ký Sự Kiện (Anatomy of an Event Log)
Khi xem xét nhật ký **Application**, ta thường gặp hai cấp độ sự kiện chính: **Information** (thông tin chung về việc sử dụng) và **Error** (chi tiết về lỗi cụ thể).



Mỗi mục trong Nhật ký sự kiện Windows bao gồm các thành phần chính sau:
* **Log Name:** Tên nhật ký (ví dụ: Application, System).
* **Source:** Phần mềm đã ghi lại sự kiện.
* **Event ID:** Mã định danh duy nhất cho sự kiện.
* **Task Category:** Mục đích hoặc cách sử dụng của sự kiện.
* **Level:** Mức độ nghiêm trọng (Information, Warning, Error, Critical, Verbose).
* **Keywords:** Từ khóa để phân loại sự kiện (ví dụ: "Audit Success", "Audit Failure"). Trường này rất hữu ích để lọc và tìm kiếm chính xác các sự kiện quan tâm.
* **User:** Tài khoản người dùng đã đăng nhập khi sự kiện xảy ra.
* **OpCode:** Xác định hoạt động cụ thể mà sự kiện báo cáo.
* **Logged:** Ngày và giờ sự kiện được ghi lại.
* **Computer:** Tên máy tính nơi sự kiện xảy ra.
* **XML Data:** Toàn bộ thông tin trên và dữ liệu bổ sung dưới định dạng XML.

## 3. Phân Tích Cụ Thể & Truy Vấn XML Tùy Chỉnh
* **Phân tích sự kiện:** Event ID là định danh duy nhất giúp tra cứu thêm thông tin từ Microsoft. Chi tiết lỗi và mã định danh quy trình (process ID) có thể được trích xuất để phân tích chính xác hơn.
* **Ví dụ Event ID 4624 (Security Log):** Biểu thị việc tạo một phiên đăng nhập thành công. Các chi tiết quan trọng bao gồm "Logon ID" (để tương quan với các sự kiện khác) và "Logon Type" (ví dụ: Service logon do SYSTEM khởi tạo).
* **Truy vấn XML tùy chỉnh:** Để phân tích hiệu quả, ta có thể tạo truy vấn XML tùy chỉnh trong Event Viewer ("Filter Current Log" -> "XML" -> "Edit Query Manually"). Ví dụ: lọc các sự kiện có "SubjectLogonId" là "0x3E7" để theo dõi hoạt động của một Logon ID cụ thể. Microsoft cung cấp tài liệu hướng dẫn về lọc XML nâng cao nếu cần hỗ trợ.
* **Ví dụ phân tích chuỗi sự kiện:**
    1.  **Event ID 4907:** Thay đổi chính sách kiểm toán (audit policy change) trên một đối tượng (ví dụ: registry key hoặc file). Điều này liên quan đến việc thay đổi SACL (System Access Control List) để ghi lại các nỗ lực truy cập.
    2.  **Chi tiết sự kiện:** Quy trình thực hiện thay đổi có thể là "SetupHost.exe" tác động lên đối tượng "bootmanager". Cần kiểm tra "NewSd" và "OldSd" (Security Descriptors) để hiểu rõ thay đổi.
    3.  **Special Logon:** Sau sự kiện đăng nhập, sự kiện "special logon" cho biết các quyền (privileges) được cấp cho người dùng, ví dụ: "SeDebugPrivilege" cho phép can thiệp vào bộ nhớ không thuộc về mình.

## 4. Các Mã Sự Kiện Windows Hữu Ích (Useful Windows Event Logs)

### Windows System Logs
* **1074:** Hệ thống tắt/khởi động lại (System Shutdown/Restart).
* **6005:** Dịch vụ Nhật ký sự kiện bắt đầu (Event log service started) - thường là lúc khởi động máy.
* **6006:** Dịch vụ Nhật ký sự kiện dừng (Event log service stopped) - thường là lúc tắt máy.
* **6013:** Thời gian hoạt động của Windows (Windows uptime) - ghi lại mỗi ngày một lần.
* **7040:** Thay đổi trạng thái dịch vụ (Service status change) - ví dụ: từ thủ công sang tự động.

### Windows Security Logs
* **1102:** Nhật ký kiểm toán bị xóa (Audit log cleared) - dấu hiệu xóa dấu vết.
* **1116, 1118, 1119, 1120:** Các sự kiện liên quan đến Windows Defender (phát hiện malware, bắt đầu/thành công/thất bại trong việc xử lý malware).
* **4624:** Đăng nhập thành công (Successful Logon).
* **4625:** Đăng nhập thất bại (Failed Logon) - nhiều lần có thể là tấn công brute-force.
* **4648:** Đăng nhập bằng thông tin xác thực rõ ràng (Explicit credentials) - dấu hiệu di chuyển ngang (lateral movement).
* **4656:** Yêu cầu xử lý đến một đối tượng (Handle requested) - giám sát truy cập tài nguyên nhạy cảm.
* **4672:** Đặc quyền đặc biệt được gán cho lần đăng nhập mới (Special Privileges Assigned).
* **4698, 4700, 4701, 4702:** Các sự kiện liên quan đến Scheduled Task (tạo, bật/tắt, cập nhật) - thường dùng để duy trì quyền truy cập (persistence).
* **4719:** Thay đổi chính sách kiểm toán hệ thống (System audit policy changed).
* **4738:** Tài khoản người dùng bị thay đổi (User account changed).
* **4771:** Xác thực trước Kerberos thất bại (Kerberos pre-authentication failed).
* **4776:** Domain controller xác thực thông tin đăng nhập.
* **5001:** Thay đổi cấu hình bảo vệ thời gian thực của Antivirus.
* **5140, 5142, 5145:** Các sự kiện liên quan đến chia sẻ mạng (Network share accessed/added/checked).
* **5157:** Windows Filtering Platform chặn kết nối.
* **7045:** Một dịch vụ được cài đặt vào hệ thống - dấu hiệu malware cài đặt dịch vụ.

**Lưu ý quan trọng:** Cần hiểu rõ hành vi "bình thường" của môi trường để phát hiện bất thường chính xác, giảm thiểu dương tính giả. Việc sử dụng giải pháp quản lý nhật ký tập trung và tương quan các loại nhật ký với nhau là rất cần thiết để có cái nhìn toàn diện về an ninh.