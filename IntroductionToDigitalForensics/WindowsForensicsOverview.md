# Tóm tắt: Tổng quan về Điều tra số trên Windows (Windows Forensics Overview)

Phần này cung cấp cái nhìn tổng quan về các dấu vết (artifacts) quan trọng và quy trình điều tra trên hệ điều hành Windows.

## 1. Hệ thống tệp NTFS (NTFS File System)
NTFS là hệ thống tệp mặc định của Windows từ bản NT 3.1, thay thế cho FAT với các tính năng bảo mật và lưu trữ tốt hơn.

### Các dấu vết quan trọng trong NTFS:
* **File Metadata:** Chứa thông tin thời gian (MAC times: Modified, Accessed, Created) và thuộc tính tệp (ẩn, chỉ đọc).
* **MFT Entries (Master File Table):** "Bản đồ" của ổ đĩa, chứa metadata của mọi tệp. Khi tệp bị xóa, entry được đánh dấu là "có sẵn" nhưng dữ liệu vẫn có thể còn trên đĩa.
* **File Slack & Unallocated Space:** Không gian chưa được phân bổ hoặc phần thừa của cluster, nơi có thể chứa dữ liệu bị xóa hoặc phân mảnh.
* **File Signatures:** Chữ ký tệp (Header) giúp nhận diện loại tệp ngay cả khi đuôi mở rộng bị đổi.
* **USN Journal:** Nhật ký ghi lại mọi thay đổi (tạo, sửa, xóa, đổi tên) đối với tệp và thư mục.
* **LNK Files (Shortcuts):** Chứa thông tin về tệp đích, thời gian truy cập, giúp xác định các tệp/chương trình được mở gần đây.
* **Prefetch Files:** Được tạo ra để tăng tốc độ khởi động ứng dụng, giúp điều tra viên biết chương trình nào đã chạy và chạy khi nào.
* **Registry Hives:** Cơ sở dữ liệu cấu hình hệ thống, chứa dấu vết của các thay đổi trái phép.
* **Shellbags:** Lưu trữ cài đặt hiển thị thư mục, giúp tái hiện lịch sử điều hướng thư mục của người dùng.
* **Thumbnail Cache:** Bộ nhớ đệm hình ảnh thu nhỏ, có thể chứa hình ảnh của các tệp đã bị xóa.
* **Recycle Bin:** Thùng rác chứa các tệp đã xóa.
* **Alternate Data Streams (ADS):** Luồng dữ liệu ẩn đi kèm với tệp, thường bị hacker lợi dụng để giấu mã độc.
* **Volume Shadow Copies:** Bản sao chụp (snapshot) hệ thống, hữu ích để khôi phục dữ liệu quá khứ.
* **Security Descriptors & ACLs:** Danh sách kiểm soát truy cập, cho biết quyền hạn của người dùng đối với tệp.



## 2. Windows Event Logs (Nhật ký sự kiện)
Lưu trữ log từ hệ thống, ứng dụng và bảo mật.

* **Vai trò:** Dùng để phát hiện xâm nhập, phân tích lỗi và theo dõi các chiến thuật tấn công (từ xâm nhập ban đầu đến leo thang đặc quyền).
* **Vị trí mặc định:** `C:\Windows\System32\winevt\logs`.
* **Công cụ:** Có thể xem trực tiếp hoặc dùng công cụ để tìm kiếm log cụ thể.



## 3. Dấu vết thực thi (Execution Artifacts)
Các bằng chứng cho thấy một chương trình hoặc quy trình đã được chạy trên hệ thống. Giúp tái hiện dòng thời gian và hành vi.

| Artifact | Mô tả & Vị trí |
| :--- | :--- |
| **Prefetch Files** | `C:\Windows\Prefetch`. Chứa tên file, đường dẫn, số lần chạy, thời gian chạy. |
| **Shimcache** | Registry (`HKLM`). Lưu thông tin tương thích ứng dụng, chứng minh file đã tồn tại và được thực thi. |
| **Amcache** | `C:\Windows\AppCompat\Programs\Amcache.hve`. Chứa đường dẫn, kích thước, chữ ký số (SHA-1) và thời gian. |
| **UserAssist** | Registry (`HKCU`). Mã hóa ROT-13. Lưu lịch sử các chương trình GUI được người dùng chạy. |
| **RunMRU Lists** | Registry. Danh sách các lệnh đã gõ trong hộp thoại "Run". |
| **Jump Lists** | Các tác vụ hoặc tệp tin thường xuyên truy cập của một ứng dụng cụ thể. |
| **Recent Items** | Thư mục chứa các tệp vừa mở gần đây. |

## 4. Dấu vết duy trì (Windows Persistence Artifacts)
Kỹ thuật kẻ tấn công dùng để duy trì quyền truy cập sau khi khởi động lại máy.

* **Registry Autorun Keys:** Các khóa Registry tự động chạy chương trình khi khởi động.
    * `Run / RunOnce` (Cả HKCU và HKLM).
    * `Winlogon\Shell`.
    * `User Shell Folders`.
* **Scheduled Tasks (Schtasks):**
    * Vị trí: `C:\Windows\System32\Tasks` (file XML).
    * Chứa thông tin: Người tạo, thời gian kích hoạt (trigger), chương trình cần chạy.
* **Services (Dịch vụ):**
    * Kẻ tấn công thường tạo dịch vụ giả mạo chạy ngầm.
    * Vị trí Registry: `HKLM\System\CurrentControlSet\Services`.



## 5. Web Browser Forensics (Điều tra trình duyệt)
Phân tích hành vi người dùng trên Internet.

* **Các artifact chính:** Lịch sử duyệt web, Cookies (phiên đăng nhập), Cache (nội dung trang web), Bookmarks, Lịch sử tải xuống (Downloads), Dữ liệu tự điền (Autofill), Mật khẩu đã lưu.

## 6. SRUM (System Resource Usage Monitor)
Tính năng từ Windows 8 trở đi, theo dõi việc sử dụng tài nguyên hệ thống.

* **Vị trí:** `C:\Windows\System32\sru\sru.db` (Cơ sở dữ liệu SQLite).
* **Khả năng phân tích:**
    * **Hồ sơ ứng dụng:** Biết được ứng dụng nào đã chạy, đường dẫn và thời gian.
    * **Tiêu thụ tài nguyên:** CPU, mạng, bộ nhớ (giúp phát hiện đào tiền ảo hoặc exfiltration dữ liệu).
    * **Ngữ cảnh người dùng:** Biết user nào đã chạy chương trình.
    * **Phát hiện Malware:** Dựa trên sự bất thường trong tiêu thụ tài nguyên hoặc các ứng dụng lạ mới xuất hiện.