# Skills Assessment: Dashboard Review & Critical Thinking

## 1. Giới thiệu & Bối cảnh
**Vai trò:** SOC Tier 1 Analyst tại Eagle.  
**Nhiệm vụ:** Giám sát các cảnh báo và sự kiện bảo mật trên Dashboard "SOC-Alerts" sau khi tiếp nhận thông tin môi trường từ chuyên gia phân tích cấp cao.

### Thông tin môi trường (Environment Intelligence)
Trước khi phân tích Dashboard, cần nắm vững các quy tắc và đặc điểm của hệ thống hiện tại:

1.  **Cấu trúc mạng:** Toàn bộ hosting đã chuyển lên Cloud. Mạng DMZ cũ đã đóng cửa (không còn server nào ở đó).
2.  **Đội ngũ IT Operation:**
    * Gồm 4 người, là những người duy nhất có đặc quyền cao.
    * **Thói quen xấu:** Thường sử dụng tài khoản `default administrator` dù đã được cảnh báo không nên.
3.  **Thiết bị đầu cuối (Endpoints):** Đã được làm cứng (hardened) theo chuẩn CIS. Có áp dụng Whitelisting ở mức độ hạn chế.
4.  **Máy trạm quản trị (PAW - Privileged Admin Workstation):**
    * Bảo mật yêu cầu **tất cả** các hoạt động quản trị (Admin activities) phải được thực hiện trên máy PAW này.
5.  **Môi trường Linux:**
    * Chủ yếu là các server cũ (legacy), rất ít hoạt động.
    * **Tài khoản Root:** Không được sử dụng. Việc remote bằng root đã bị chặn (audit finding). Người dùng phải dùng `sudo` để leo thang đặc quyền.
6.  **Quy ước đặt tên & Tài khoản dịch vụ (Service Accounts):**
    * Tài khoản dịch vụ luôn có chuỗi `-svc` trong tên.
    * Mật khẩu dài và phức tạp.
    * Mục đích: Chỉ thực hiện tác vụ cụ thể (thường là chạy service nội bộ trên máy), **không dùng để đăng nhập tương tác**.

---

## 2. Phân tích Dashboard "SOC-Alerts"

Dưới đây là phân tích chi tiết cho từng Visualization dựa trên bối cảnh đã cung cấp:

### Visualization 1: Failed logon attempts (All users)
* **Mô tả:** Biểu đồ hiển thị các lần đăng nhập thất bại.
* **Dữ liệu quan sát:** Có sự bất thường liên quan đến tài khoản `sql-svc1`.
* **Phân tích tư duy (Critical Thinking):**
    * Tài khoản dịch vụ (`-svc`) thường được cấu hình cứng trong các ứng dụng/script với mật khẩu phức tạp và ít khi thay đổi thủ công.
    * Việc `sql-svc1` bị lỗi đăng nhập có thể do cấu hình sai (sai mật khẩu trong script) hoặc một cuộc tấn công Brute-force nhắm vào tài khoản dịch vụ.
    * *Hành động:* Cần kiểm tra nguồn IP thực hiện đăng nhập.

### Visualization 2: Failed logon attempts (Disabled user)
* **Mô tả:** Giám sát đăng nhập vào các tài khoản đã bị vô hiệu hóa.
* **Dữ liệu quan sát:** Người dùng "Anni" cố gắng xác thực dù tài khoản đã bị tắt.
* **Phân tích tư duy (Critical Thinking):**
    * Tại sao một tài khoản bị vô hiệu hóa lại cố gắng đăng nhập?
    * Khả năng 1: Nhân viên cũ cố gắng truy cập lại.
    * Khả năng 2: Một tác vụ tự động (Scheduled Task) cũ vẫn đang chạy dưới quyền user này.
    * Khả năng 3: Kẻ tấn công đang cố gắng dò thông tin xác thực cũ.

### Visualization 3: Failed logon attempts (Admin users only)
* **Mô tả:** Đăng nhập thất bại của tài khoản Admin.
* **Gợi ý (Hint):** Kiểm tra xem các sự kiện này có diễn ra trên **PAW** hoặc **Domain Controllers** hay không.
* **Phân tích tư duy (Critical Thinking):**
    * Theo chính sách: Admin chỉ được hoạt động trên PAW.
    * Nếu các lần đăng nhập thất bại này đến từ các máy trạm thông thường (Workstations) hoặc từ mạng bên ngoài, đây là dấu hiệu vi phạm chính sách hoặc tấn công mạng.

### Visualization 4: RDP logon for service account
* **Mô tả:** Đăng nhập RDP (Remote Desktop) thành công của tài khoản dịch vụ.
* **Dữ liệu quan sát:** Tài khoản dịch vụ đang thực hiện RDP.
* **Phân tích tư duy (Critical Thinking):**
    * **Quy tắc:** Tài khoản dịch vụ (`-svc`) chỉ chạy service cục bộ (run services locally).
    * **Bất thường:** Tài khoản dịch vụ **không bao giờ** nên được dùng để RDP (đăng nhập tương tác từ xa).
    * *Kết luận:* Đây là hành vi cực kỳ đáng ngờ. Có thể kẻ tấn công đã chiếm được tài khoản dịch vụ và đang cố gắng di chuyển ngang (lateral movement) trong hệ thống.

### Visualization 5: User added or removed from a local group
* **Mô tả:** Thay đổi thành viên trong nhóm "Administrators".
* **Dữ liệu quan sát:** Một Admin đã thêm một đối tượng (chỉ hiển thị dưới dạng mã SID, chưa phân giải được tên) vào nhóm Admin.
* **Quyết định (Escalate vs Consult):**
    * Vì đội IT Operation (4 người) là những người duy nhất có quyền cao, khả năng cao là họ thực hiện việc này.
    * Tuy nhiên, việc thêm một SID lạ (có thể là user đã bị xóa hoặc user từ domain khác không xác định) là rủi ro.
    * *Hành động:* **Consult (Tham vấn)** với bộ phận IT Operations trước để xác nhận xem đây có phải là thay đổi hợp lệ hay không. Nếu họ không làm, lập tức Escalate.

### Visualization 6: Admin logon not from PAW
* **Mô tả:** Admin đăng nhập thành công nhưng không từ máy PAW.
* **Dữ liệu quan sát:** Vi phạm chính sách PAW.
* **Quyết định (Escalate vs Consult):**
    * Chúng ta biết đội IT Operation có "thói quen xấu" và thường làm trái quy định.
    * *Hành động:* **Consult (Tham vấn)** với IT Operations để nhắc nhở về chính sách và xác nhận hành động đó là của họ. Tuy nhiên, cần giám sát chặt chẽ vì kẻ tấn công cũng có thể lợi dụng thói quen này để ẩn mình.

### Visualization 7: SSH Logins
* **Mô tả:** Đăng nhập SSH vào Linux.
* **Dữ liệu quan sát:** Có hoạt động liên quan đến tài khoản `root`.
* **Phân tích tư duy (Critical Thinking):**
    * **Quy tắc:** Tài khoản `root` bị cấm remote login, server Linux là đồ cũ ít hoạt động.
    * Nếu thấy đăng nhập SSH thành công bằng `root`, nghĩa là cơ chế bảo mật đã bị vô hiệu hóa hoặc bị vượt qua.
    * Đây là một cảnh báo mức độ nghiêm trọng cao (High Severity).

---

## 3. Tổng kết hành động cho Analyst Tier 1

1.  **Xác minh bối cảnh:** Luôn đối chiếu sự kiện với thông tin tình báo môi trường (ví dụ: quy tắc đặt tên `-svc`, chính sách PAW).
2.  **Phân loại sự cố:**
    * *Vi phạm chính sách/Thói quen xấu:* Tham vấn (Consult) đội IT Ops.
    * *Dấu hiệu tấn công rõ ràng (RDP bằng service account, Root login):* Leo thang (Escalate) lên Tier 2/3.
3.  **Ghi chép:** Ghi lại đầy đủ các quan sát về `sql-svc1`, `Anni`, và các vi phạm PAW vào ticket quản lý sự cố.