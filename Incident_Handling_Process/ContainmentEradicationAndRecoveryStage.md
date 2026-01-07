# Giai đoạn Ngăn chặn, Loại bỏ và Phục hồi

Khi quá trình điều tra hoàn tất và chúng ta đã hiểu rõ loại sự cố cũng như tác động của nó đối với doanh nghiệp (dựa trên tất cả các manh mối thu thập được và thông tin được tổng hợp trong dòng thời gian), đã đến lúc bước vào giai đoạn ngăn chặn để ngăn sự cố gây thêm thiệt hại.

---

## 1. Ngăn chặn (Containment)

Trong giai đoạn này, chúng ta thực hiện hành động để ngăn chặn sự lây lan của sự cố. Chúng ta chia các hành động thành **ngăn chặn ngắn hạn** và **ngăn chặn dài hạn**.

> **Quan trọng:** Các hành động ngăn chặn phải được phối hợp và thực hiện đồng bộ trên tất cả các hệ thống cùng một lúc. Nếu không, chúng ta có nguy cơ "đánh động" kẻ tấn công rằng chúng đang bị truy đuổi, khi đó chúng có thể thay đổi kỹ thuật và công cụ để duy trì sự tồn tại trong môi trường mạng.

### Ngăn chặn ngắn hạn (Short-term containment)
Trong ngăn chặn ngắn hạn, các hành động được thực hiện sẽ để lại dấu vết tối thiểu trên các hệ thống bị ảnh hưởng. Những hành động này nhằm hạn chế thiệt hại và giúp có thêm thời gian để xây dựng một chiến lược khắc phục cụ thể hơn.

* **Các hành động có thể bao gồm:**
    * Đưa hệ thống vào một VLAN riêng biệt/cô lập.
    * Rút dây mạng khỏi (các) hệ thống.
    * Sửa đổi tên miền DNS máy chủ C2 (Command & Control) của kẻ tấn công trỏ về một hệ thống do chúng ta kiểm soát hoặc một địa chỉ không tồn tại.
* **Điều tra số & Bằng chứng:** Vì chúng ta giữ cho các hệ thống không bị thay đổi (càng nhiều càng tốt), đây là cơ hội để lấy các bản sao chép pháp y (forensic images) và bảo tồn bằng chứng nếu việc này chưa được thực hiện trong quá trình điều tra (bước này còn được gọi là tiểu giai đoạn **sao lưu** của giai đoạn ngăn chặn).
* **Quyền hạn:** Nếu hành động ngăn chặn ngắn hạn yêu cầu tắt hệ thống, chúng ta phải đảm bảo việc này đã được thông báo cho doanh nghiệp và được cấp các quyền thích hợp.

### Ngăn chặn dài hạn (Long-term containment)
Trong ngăn chặn dài hạn, chúng ta tập trung vào các hành động và thay đổi mang tính bền vững. Trong khi thực hiện các hoạt động này, cần cập nhật thông tin cho doanh nghiệp và các bên liên quan.

* **Các hành động bao gồm:**
    * Thay đổi mật khẩu người dùng.
    * Áp dụng các quy tắc tường lửa (firewall rules).
    * Cài đặt hệ thống phát hiện xâm nhập máy chủ (HIDS).
    * Áp dụng các bản vá hệ thống.
    * Tắt các hệ thống bị ảnh hưởng.

*Lưu ý: Chỉ vì một hệ thống đã được vá không có nghĩa là sự cố đã kết thúc. Các bước Loại bỏ, Phục hồi và các hoạt động sau sự cố vẫn đang chờ xử lý.*

---

## 2. Loại bỏ (Eradication)

Sau khi sự cố đã được ngăn chặn, việc loại bỏ là cần thiết để triệt tiêu cả nguyên nhân gốc rễ của sự cố và những gì còn sót lại, đảm bảo rằng kẻ địch đã hoàn toàn bị loại khỏi hệ thống và mạng lưới.

* **Các hoạt động chính:**
    * Gỡ bỏ phần mềm độc hại (malware) đã bị phát hiện khỏi hệ thống.
    * Xây dựng lại (rebuild) một số hệ thống.
    * Khôi phục các hệ thống khác từ bản sao lưu sạch.
    * **Gia cố hệ thống (Hardening):** Trong giai đoạn loại bỏ, chúng ta có thể mở rộng các hoạt động ngăn chặn trước đó bằng cách áp dụng thêm các bản vá (những bản chưa cần thiết ngay lập tức trong lúc ngăn chặn). Các hoạt động gia cố hệ thống thường được thực hiện trong giai đoạn này (không chỉ trên hệ thống bị ảnh hưởng mà còn trên toàn mạng trong một số trường hợp).

---

## 3. Phục hồi (Recovery)

Trong giai đoạn phục hồi, chúng ta đưa các hệ thống trở lại hoạt động bình thường. Tất nhiên, doanh nghiệp cần xác minh rằng hệ thống thực sự hoạt động như mong đợi và chứa đầy đủ dữ liệu cần thiết. Khi mọi thứ đã được xác minh, các hệ thống này sẽ được đưa trở lại môi trường sản xuất (production).

### Giám sát & Ghi nhật ký (Logging)
Tất cả các hệ thống được khôi phục sẽ phải chịu sự **giám sát và ghi nhật ký chặt chẽ** sau sự cố, vì các hệ thống từng bị xâm nhập thường có xu hướng trở thành mục tiêu một lần nữa nếu kẻ địch giành lại được quyền truy cập vào môi trường trong thời gian ngắn.

**Các sự kiện đáng ngờ điển hình cần giám sát:**
* Đăng nhập bất thường (ví dụ: tài khoản người dùng hoặc dịch vụ chưa từng đăng nhập ở đó trước đây).
* Các tiến trình (process) bất thường.
* Thay đổi trong Registry tại các vị trí thường bị phần mềm độc hại sửa đổi.

### Tiếp cận theo giai đoạn
Giai đoạn phục hồi trong một số sự cố lớn có thể mất nhiều tháng, vì nó thường được tiếp cận theo từng giai đoạn:
1.  **Các giai đoạn đầu:** Tập trung vào việc tăng cường bảo mật tổng thể để ngăn chặn các sự cố trong tương lai thông qua các "thắng lợi nhanh chóng" (quick wins) và loại bỏ các điểm yếu dễ bị khai thác nhất (low-hanging fruit).
2.  **Các giai đoạn sau:** Tập trung vào các thay đổi lâu dài, vĩnh viễn để giữ cho tổ chức được bảo mật nhất có thể.