# Tóm tắt chi tiết: Event Tracing for Windows (ETW)

## 1. Tổng quan về ETW
* **Định nghĩa:** Là một cơ chế theo dõi (tracing) hiệu suất cao, đa mục đích được tích hợp sẵn trong nhân (kernel) của Windows.
* **Mục tiêu:** Cung cấp khả năng giám sát thời gian thực cho cả ứng dụng chế độ người dùng (user-mode) và trình điều khiển chế độ nhân (kernel-mode).
* **Ưu điểm:** * Tốc độ cực nhanh, ít ảnh hưởng đến hiệu suất hệ thống.
    * Dữ liệu telemetry vượt xa các log truyền thống (Sysmon/Event Logs).
    * Khả năng mở rộng và tùy chỉnh cao thông qua các Provider.

## 2. Kiến trúc & Thành phần (Mô hình Publish-Subscribe)
ETW hoạt động dựa trên 3 thành phần chính:

| Thành phần | Vai trò chính | Ví dụ |
| :--- | :--- | :--- |
| **Controllers** | Khởi tạo, dừng các phiên trace (sessions) và kích hoạt/vô hiệu hóa Providers. | `logman.exe` |
| **Providers** | Tạo ra các sự kiện (Events). Có 4 loại: MOF, WPP, Manifest-based và TraceLogging. | Microsoft-Windows-Kernel-Process |
| **Consumers** | Đăng ký nhận sự kiện để phân tích thời gian thực hoặc đọc từ file log. | Message Analyzer, PowerShell |

* **Channels:** Các container logic để tổ chức và lọc sự kiện (ví dụ: Operational, Admin).
* **ETL Files (.etl):** Định dạng file lưu trữ dữ liệu trace để phân tích ngoại tuyến hoặc pháp y.

## 3. Các công cụ tương tác chính
* **logman.exe (CLI):** Công cụ mặc định mạnh mẽ nhất.
    * `logman query -ets`: Xem các phiên trace đang chạy.
    * `logman query "SessionName" -ets`: Xem chi tiết một phiên (Provider nào đang tham gia).
    * `logman query providers`: Liệt kê tất cả Provider có sẵn trên hệ thống.
* **Performance Monitor (GUI):** Giao diện đồ họa để quản lý và trực quan hóa các phiên trace.
* **ETW Explorer:** Công cụ bên thứ ba giúp xem metadata của các Provider.

## 4. Các Provider quan trọng cho Bảo mật (Blue Team)
Việc theo dõi các Provider này giúp phát hiện hành vi tấn công tinh vi:
* **Kernel-Process/File/Network/Registry:** Giám sát mọi hoạt động cốt lõi của hệ điều hành (tiêm mã, truy cập file trái phép, kết nối C2).
* **PowerShell:** Theo dõi thực thi script block, phát hiện script độc hại.
* **DotNETRuntime:** Phát hiện nạp assembly lạ hoặc khai thác lỗ hổng .NET.
* **DNS-Client:** Theo dõi DNS tunneling hoặc các yêu cầu phân giải tên miền khả nghi.
* **Antimalware-Service/Protection:** Giám sát tình trạng và sự can thiệp vào trình diệt virus.

## 5. Provider bị hạn chế (Restricted Providers)
* **Microsoft-Windows-Threat-Intelligence (TI):** Cung cấp dữ liệu cực kỳ chi tiết về các mối đe dọa.
* **Cơ chế bảo vệ:** Chỉ những tiến trình có chứng chỉ **PPL (Protected Process Light)** — thường là các phần mềm Anti-Malware đã được Microsoft xác thực — mới có quyền truy cập trực tiếp.
* **Ý nghĩa:** Đây là nguồn telemetry "vàng" cho điều tra pháp y (DFIR) vì nó ghi lại các hành vi mà các lớp bảo vệ khác có thể bỏ lỡ.

---
**Ghi chú:** ETW là "xương sống" cho nhiều giải pháp EDR hiện đại. Việc nắm vững cách truy vấn và lọc dữ liệu từ ETW là kỹ năng thiết yếu để vượt qua giới hạn của các bản ghi log thông thường.