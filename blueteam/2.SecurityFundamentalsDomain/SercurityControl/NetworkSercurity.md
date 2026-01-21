# Tóm tắt Chi tiết: Network Security (An ninh Mạng)

Tài liệu này giới thiệu các biện pháp phòng thủ mạng cơ bản, đóng vai trò là nền tảng kiến thức sẽ được mở rộng trong các phần sau của khóa học BTL1.

## 1. Hệ thống Phát hiện Xâm nhập Mạng (NIDS - Network Intrusion Detection Systems)
**Định nghĩa:** NIDS là hệ thống (phần mềm hoặc thiết bị vật lý) giám sát lưu lượng mạng để tạo cảnh báo cho các chuyên gia phân tích điều tra.

**Các vị trí triển khai:**
* **Inline (Trực tiếp):** Hệ thống nằm trực tiếp trên đường đi của lưu lượng mạng (tất cả lưu lượng phải đi qua nó). Khi ở vị trí này, theo định nghĩa, nó thực chất trở thành Hệ thống Ngăn chặn (NIPS) vì có khả năng chặn hoặc ngắt kết nối.
* **Network Tap:** NIDS kết nối vào mạng thông qua một điểm đấu nối vật lý vào dây cáp.
* **Passive (Thụ động):** NIDS kết nối vào cổng **SPAN** trên thiết bị mạng. Cổng này sao chép (mirror) toàn bộ lưu lượng đi qua thiết bị và gửi bản sao tới NIDS, giúp giám sát mà không ảnh hưởng luồng mạng chính.

**Mục đích chính:** Tạo cảnh báo để con người điều tra. NIDS thuần túy chỉ phát hiện và báo động, không tự động chặn.

---

## 2. Hệ thống Ngăn chặn Xâm nhập Mạng (NIPS - Network Intrusion Prevention Systems)
**Sự khác biệt với NIDS:**
* Trong khi NIDS chỉ phát hiện, **NIPS** có khả năng tự động thực hiện các hành động phòng thủ dựa trên hoạt động đã nhận diện.
* Tên gọi đã thể hiện rõ: "Prevention" (Ngăn chặn) so với "Detection" (Phát hiện).

**Ví dụ hoạt động:**
Nếu một hệ thống nội bộ bắt đầu quét (scan) các hệ thống khác (hành vi bất thường), NIPS có thể được lập trình để **tự động chặn** giao tiếp từ hệ thống đó và tạo cảnh báo.

---

## 3. Tường lửa (Firewalls)
**Chức năng:** Tách biệt các phần của mạng để tạo ra các vùng riêng tư bằng cách hạn chế lưu lượng vào/ra. Ví dụ: Chặn kết nối từ Internet vào mạng nội bộ, chỉ cho phép các giao thức như HTTP, HTTPS, DNS.

**Ba loại tường lửa chính:**
1.  **Standard Firewalls (Tiêu chuẩn):** Chạy trên phần cứng chuyên dụng, đặt tại các điểm chốt chặn của mạng.
2.  **Local Firewalls (Cục bộ):** Phần mềm chạy trên thiết bị đầu cuối (ví dụ: Windows Firewall).
3.  **Web Application Firewalls (WAF):** Phần mềm nằm trên máy chủ web để bảo vệ các trang web hoặc ứng dụng web.

---

## 4. Giám sát Sự kiện (Event Monitoring)
Các thiết bị mạng tạo ra nhật ký (logs) và gửi về hệ thống **SIEM**. SIEM cung cấp bảng điều khiển (dashboard) để phân tích và phản ứng.

**Các ví dụ cụ thể:**
* **Web Proxy Logs:** Ghi lại danh sách các trang web nhân viên truy cập. Kết hợp với danh sách đen (blacklist) để cảnh báo khi nhân viên vào trang web độc hại hoặc cấm.
* **Perimeter Firewalls (Tường lửa vành đai):** Phát hiện sớm các cuộc quét cổng (port scanning) hoặc tấn công từ chối dịch vụ (DDoS) do lượng yêu cầu lớn đập vào tường lửa.

---

## 5. Kiểm soát Truy cập Mạng (NAC - Network Access Control)
**Chức năng:** Ngăn chặn các thiết bị lạ (rogue) hoặc không tuân thủ quy định kết nối vào mạng.

**Cơ chế hoạt động:**
* Yêu cầu thiết bị phải có bản cập nhật bảo mật mới nhất và đang chạy Anti-virus mới được phép kết nối.
* Thường dùng cho môi trường **BYOD** (Mang thiết bị cá nhân) hoặc mạng Khách (Guest networks).

**Ví dụ thực tế:**
Khi bạn đến nhà hàng hoặc sân bay và thấy Wifi miễn phí nhưng yêu cầu phải đăng nhập qua một trang chào (Captive Portal) mới dùng được mạng. Đó chính là NAC đang hoạt động để quản lý phiên truy cập.