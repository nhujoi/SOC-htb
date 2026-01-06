# Giai đoạn Chuẩn bị (Preparation Stage) - Quy trình Xử lý Sự cố

Giai đoạn Chuẩn bị là nền tảng quan trọng nhất. Đây là lúc tổ chức xây dựng năng lực để sẵn sàng đối phó khi sự cố xảy ra.

---

## 1. Mục tiêu chính

Giai đoạn này có hai mục tiêu riêng biệt:
1.  **Thiết lập năng lực xử lý:** Xây dựng đội ngũ, quy trình và công cụ.
2.  **Ngăn chặn và bảo vệ:** Triển khai các biện pháp an ninh như Hardening (làm cứng hệ thống), phân tầng Active Directory, MFA, quản lý đặc quyền...
    * *Lưu ý:* Dù việc bảo vệ không phải nhiệm vụ trực tiếp của đội xử lý sự cố (Incident Handling Team), nhưng nó là yếu tố then chốt giúp đội ngũ này thành công.

---

## 2. Điều kiện tiên quyết (Prerequisites)

Để hoạt động hiệu quả, cần đảm bảo 4 yếu tố:
* **Nhân sự:** Đội ngũ có kỹ năng (có thể thuê ngoài, nhưng nội bộ bắt buộc phải có kiến thức cơ bản).
* **Lực lượng lao động:** Được đào tạo nhận thức bảo mật (Security Awareness).
* **Chính sách & Tài liệu:** Rõ ràng, cập nhật.
* **Công cụ:** Phần cứng và phần mềm đầy đủ.

---

## 3. Chính sách & Tài liệu (Policies & Documentation)

Tài liệu cần được chuẩn bị sẵn ("thời bình") và cập nhật liên tục:

### A. Thông tin liên hệ & Vai trò
* Danh sách và vai trò của thành viên đội xử lý sự cố.
* Liên hệ các bên liên quan: Pháp lý, Ban quản lý, IT Support, Truyền thông, Cảnh sát (Law enforcement), ISP, và Đội ứng cứu thuê ngoài (nếu có).

### B. Thông tin Hạ tầng Kỹ thuật
* Sơ đồ mạng (Network diagrams).
* Cơ sở dữ liệu quản lý tài sản (Asset management).
* Cấu hình chuẩn (Baselines) của hệ thống từ trạng thái sạch (Golden Image).

### C. Tài nguyên & Quy trình đặc biệt
* **Tài khoản khẩn cấp (Break-glass Accounts):**
    * Các tài khoản có đặc quyền cao, dùng để quản trị hệ thống quan trọng khi có sự cố.
    * *Quy trình:* Bình thường bị vô hiệu hóa -> Bật khi có sự cố -> Vô hiệu hóa và đổi mật khẩu ngay sau khi xong.
* **Quy trình mua sắm khẩn cấp:**
    * Cơ chế duyệt mua nhanh (phần cứng/phần mềm) mà không qua quy trình đấu thầu rườm rà.
    * *Mục đích:* Tránh việc phải chờ hàng tuần chỉ để mua một công cụ $500 khi đang "dầu sôi lửa bỏng".
* **Cheat sheets:** Tài liệu hướng dẫn nhanh cho điều tra/pháp y kỹ thuật số.

### D. Báo cáo & Ghi chép (Reporting)
* **Nguyên tắc:** Ghi chép ngay khi điều tra. Trả lời các câu hỏi: *Ai, Cái gì, Khi nào, Ở đâu, Tại sao, Như thế nào*.
* **Yêu cầu:** Phải có dấu thời gian (timestamps) cho mọi hành động.

---

## 4. Công cụ (Tools - Software & Hardware)

### A. Khái niệm "Jump Bag"
Nhiều công cụ cần được gói gọn trong một túi phản ứng nhanh (**Jump Bag**) để có thể mang đi ngay lập tức, tránh mất thời gian thu gom.

### B. Phần cứng (Hardware)
* **Laptop/Workstation điều tra:** Máy tính riêng biệt để phân tích dữ liệu và malware (Antivirus phải bị tắt trên máy này).
* **Write Blockers (Thiết bị chặn ghi):** Đảm bảo tính toàn vẹn của bằng chứng khi sao chép ổ cứng.
* **Vật tư:** Ổ cứng dự phòng (để image), cáp mạng, switch, cáp nguồn, tua vít, nhíp...

### C. Phần mềm (Software)
* Công cụ thu thập và phân tích ảnh pháp y (Forensic image acquisition).
* Công cụ phân tích bộ nhớ (Memory capture) và Log.
* Công cụ bắt gói tin mạng (Network capture).
* Phần mềm mã hóa (Encryption software).

---

## 5. Lưu ý Quan trọng: Hạ tầng Độc lập

> **CẢNH BÁO:**
> * **Hệ thống quản lý sự cố (Ticket tracking) và Tài liệu** phải hoàn toàn **ĐỘC LẬP** với hạ tầng của tổ chức.
> * **Kênh liên lạc** cũng phải tách biệt (Out-of-band communication).
>
> **Lý do:** Phải luôn giả định rằng toàn bộ hệ thống/domain của tổ chức đã bị xâm nhập (Compromised). Kẻ tấn công có thể đang đọc email hoặc theo dõi hệ thống nội bộ của bạn.

---

# Các Biện pháp Bảo vệ

Mặc dù việc bảo vệ hệ thống không hoàn toàn thuộc trách nhiệm của đội xử lý sự cố (Incident Handling Team), nhưng họ cần nắm rõ các hoạt động này để hiểu được mức độ tinh vi của cuộc tấn công và biết nơi tìm kiếm bằng chứng.

Dưới đây là các biện pháp bảo vệ được khuyến nghị, có tác động giảm thiểu rủi ro cao đối với đa số các mối đe dọa.

## 1. DMARC (Bảo vệ Email)
* **Cơ chế:** DMARC là cơ chế bảo vệ email chống lừa đảo (phishing) được xây dựng dựa trên SPF và DKIM.
* **Chức năng:** Nó từ chối các email "giả vờ" xuất phát từ tổ chức của bạn (spoofing) trước khi chúng đến tay người nhận.
* **Triển khai:** DMARC dễ triển khai và chi phí thấp, nhưng bắt buộc phải kiểm thử kỹ lưỡng để tránh chặn nhầm email hợp lệ.
* **Nâng cao:** Có thể mở rộng để chặn email từ các tên miền bên ngoài nếu chúng thất bại kiểm tra DMARC, nhưng cần thận trọng vì rủi ro dương tính giả cao (thường gặp với các dịch vụ gửi email thay mặt).

## 2. Endpoint Hardening & EDR (Làm cứng thiết bị đầu cuối)
Thiết bị đầu cuối là điểm xâm nhập chính của hầu hết các cuộc tấn công. Các tiêu chuẩn làm cứng phổ biến nên dùng làm nền tảng là CIS và Microsoft baselines.

**Các hành động quan trọng:**
* **Giao thức mạng:** Vô hiệu hóa LLMNR và NetBIOS.
* **Quản lý quyền:** Triển khai LAPS và loại bỏ quyền quản trị (admin) khỏi người dùng thông thường.
* **PowerShell:** Vô hiệu hóa hoặc cấu hình ở chế độ "ConstrainedLanguage".
* **Microsoft Defender:** Bật các quy tắc Giảm bề mặt tấn công (ASR - Attack Surface Reduction).
* **Whitelisting (Danh sách cho phép):**
    * Nếu khó triển khai toàn diện, ít nhất hãy chặn thực thi từ các thư mục người dùng có quyền ghi (Downloads, Desktop, AppData...).
    * Chặn các loại script như .hta, .vbs, .cmd, .bat, .js.
    * Chú ý chặn cả LOLBins (các tệp nhị phân có sẵn trong hệ điều hành) vì chúng thường được dùng để vượt qua whitelisting.
* **Host-based Firewalls:** Tối thiểu nên chặn giao tiếp giữa các máy trạm (workstation-to-workstation) và chặn lưu lượng đi ra (outbound) từ các LOLBins.
* **EDR:** Nên chọn sản phẩm tích hợp với AMSI để có thể kiểm tra nội dung của các script bị làm rối (obfuscated) trước khi thực thi.

## 3. Network Protection (Bảo vệ Mạng)
* **Phân đoạn mạng (Segmentation):** Cô lập các hệ thống quan trọng và ngăn tài nguyên nội bộ tiếp xúc trực tiếp với Internet (trừ khi đặt trong DMZ).
* **IDS/IPS:** Hệ thống phát hiện/ngăn chặn xâm nhập phát huy hiệu quả nhất khi thực hiện giải mã SSL/TLS để kiểm tra nội dung gói tin, thay vì chỉ dựa vào uy tín địa chỉ IP.
* **Kiểm soát truy cập:**
    * Sử dụng chuẩn **802.1x** để giảm rủi ro từ thiết bị cá nhân (BYOD) hoặc thiết bị lạ kết nối vào mạng.
    * Với môi trường Cloud (Azure AD/Microsoft Entra ID), sử dụng **Conditional Access** để đảm bảo chỉ thiết bị do công ty quản lý mới được truy cập tài nguyên.

## 4. Quản lý Danh tính, MFA & Mật khẩu
* **Thực trạng:** Đánh cắp thông tin đăng nhập của người dùng đặc quyền là con đường leo thang phổ biến nhất trong môi trường Active Directory.
* **Vấn đề mật khẩu:** Mật khẩu "phức tạp nhưng yếu" (ví dụ: "Password1!") rất dễ đoán và có trong nhiều danh sách từ điển tấn công.
* **Khuyến nghị:** Sử dụng **cụm mật khẩu (passphrases)** dài, dễ nhớ nhưng khó đoán (ví dụ: "i LIK3 my coffeE warm").
* **MFA:** Bắt buộc áp dụng xác thực đa yếu tố cho mọi quyền truy cập quản trị vào tất cả ứng dụng và thiết bị.

## 5. Các hoạt động bảo vệ khác

### Vulnerability Scanning (Quét lỗ hổng)
* Thực hiện quét liên tục và vá các lỗ hổng mức "High" và "Critical".
* Nếu không thể vá, bắt buộc phải phân đoạn hệ thống bị ảnh hưởng.

### User Awareness Training (Đào tạo người dùng)
* Đào tạo người dùng nhận biết và báo cáo hành vi đáng ngờ giúp giảm đáng kể số lượng vụ xâm nhập thành công.
* Nên thực hiện các bài kiểm tra bất ngờ định kỳ (email phishing giả lập, thả USB tại văn phòng...).

### Active Directory Security Assessment (Đánh giá AD)
* Cần thực hiện đánh giá dưới góc nhìn của kẻ tấn công để tìm các cấu hình sai hoặc lỗ hổng leo thang.
* Mục tiêu là loại bỏ các "chiến thắng dễ dàng" (low-hanging fruit), buộc kẻ tấn công phải hoạt động nhiều hơn, từ đó tăng khả năng bị phát hiện.
* Không nên giả định rằng quản trị viên hệ thống đã biết hết các lỗi của AD.

### Purple Team Exercises (Diễn tập Purple Team)
* Đây là các bài đánh giá bảo mật mà đội tấn công (Red team) thông báo liên tục cho đội phòng thủ (Blue team) về hành động và phát hiện của họ.
* Giúp xác định lỗ hổng và kiểm tra năng lực giám sát, phát hiện, cũng như quy trình phản ứng của đội Blue team.