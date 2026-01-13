Lệnh Get-WinEvent trong PowerShell

## 1. Tổng quan
* **Vai trò:** Là công cụ mạnh mẽ để truy vấn nhật ký sự kiện Windows (Event Logs), Sysmon và Event Tracing for Windows (ETW) trên quy mô lớn.
* **Tầm quan trọng:** Giúp xử lý khối lượng log khổng lồ (hàng triệu bản ghi) mà các công cụ đồ họa như Event Viewer không thể đáp ứng hiệu quả.

## 2. Các tham số liệt kê cơ bản
| Tham số | Chức năng | Ví dụ |
| :--- | :--- | :--- |
| `-ListLog *` | Liệt kê tất cả các bản ghi có trên hệ thống cùng thuộc tính (số lượng bản ghi, trạng thái, kích thước). | `Get-WinEvent -ListLog *` |
| `-ListProvider *` | Liệt kê các nguồn cung cấp sự kiện (Providers) và liên kết của chúng với các log. | `Get-WinEvent -ListProvider *` |
| `-LogName` | Chỉ định tên log cụ thể để truy vấn (System, Security, Application...). | `-LogName 'System'` |
| `-MaxEvents` | Giới hạn số lượng sự kiện trả về để tăng tốc độ truy vấn. | `-MaxEvents 50` |
| `-Oldest` | Trả về các sự kiện theo thứ tự thời gian từ cũ nhất đến mới nhất. | `-Oldest -MaxEvents 30` |
| `-Path` | Đọc dữ liệu trực tiếp từ các tệp nhật ký đã xuất (`.evtx`). | `-Path 'C:\Logs\Sample.evtx'` |

## 3. Kỹ thuật lọc sự kiện (Filtering)
Lọc dữ liệu tại nguồn là cách hiệu quả nhất để xử lý log khối lượng lớn.

### A. FilterHashtable (Phổ biến nhất)
Sử dụng bảng băm (Hashtable) để định nghĩa các điều kiện như ID sự kiện, thời gian, hoặc đường dẫn.
* **Ví dụ:** Lọc Sysmon ID 1 (Tạo tiến trình) và ID 3 (Kết nối mạng).
* **Lọc theo thời gian:** Có thể sử dụng biến `$startDate` và `$endDate` để giới hạn khoảng thời gian điều tra.

### B. Lọc qua XML và XPath
Cung cấp khả năng lọc cực kỳ chi tiết vào sâu bên trong nội dung sự kiện.
* **`-FilterXml`:** Sử dụng cấu trúc XML để lọc (Ví dụ: Tìm các tiến trình nạp `clr.dll` hoặc `mscoree.dll` qua Sysmon ID 7).
* **`-FilterXPath`:** Sử dụng cú pháp XPath để truy vấn các thuộc tính cụ thể (Ví dụ: Tìm lệnh `reg.exe` được dùng để chấp nhận EULA của Sysinternals).

## 4. Phân tích thuộc tính (Property Values)
Mỗi sự kiện có một mảng các thuộc tính (`Properties`) ẩn chứa thông tin chi tiết:
* **Truy cập thuộc tính:** Sử dụng `$_.Properties[index].Value` để lấy dữ liệu.
* **Ví dụ:** Trong Sysmon ID 1, thuộc tính tại chỉ số **21** thường tương ứng với `ParentCommandLine` (Lệnh của tiến trình cha).
* **Ứng dụng:** Tìm kiếm các lệnh PowerShell bị mã hóa bằng cách lọc chuỗi `-enc` trong thuộc tính dòng lệnh của tiến trình cha.

## 5. Ứng dụng trong Threat Hunting
* **Phát hiện C2:** Lọc các sự kiện Sysmon ID 1 và 3 xảy ra gần nhau để tìm các binary lạ kết nối ra ngoài internet.
* **Phân tích XML thủ công:** Chuyển đổi sự kiện sang dạng XML (`$_.ToXml()`) để trích xuất các trường dữ liệu tùy chỉnh như `SourceIP`, `DestinationIP` hoặc `ProcessGuid`.


Get-WinEvent -LogName 'System' -MaxEvents 50 | Select-Object TimeCreated, ID, ProviderName, LevelDisplayName, Message | Format-Table -AutoSize

Get-WinEvent -Path "C:\Tools\chainsaw\EVTX-ATTACK-SAMPLES\Lateral Movement\*.evtx" | Where-Object { $_.Id -eq 5142 -and $_.Message -like "*PRINT*" } | Select-Object TimeCreated, Message | Format-Table -AutoSize