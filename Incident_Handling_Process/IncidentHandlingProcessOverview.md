# Tổng quan về Quy trình Xử lý Sự cố (Incident Handling Process)

## 1. Định nghĩa & Đặc điểm

* **Mục đích:** Xác định khả năng của tổ chức trong việc chuẩn bị, phát hiện và phản ứng với các sự kiện độc hại.
* **Tính chất:** Quy trình này **không tuyến tính (linear)** mà có **tính chu kỳ (cyclic)**. Khi bằng chứng mới được phát hiện trong quá trình điều tra, các bước tiếp theo cũng có thể thay đổi.
* **Mối liên hệ:** Mặc dù bổ trợ cho nhau, các giai đoạn xử lý sự cố không tương ứng 1-1 với các giai đoạn của Cyber Kill Chain.

---

## 2. Các giai đoạn chính (Theo NIST)

Theo NIST, quy trình gồm 4 giai đoạn riêng biệt. Chuyên viên xử lý sự cố dành phần lớn thời gian ở hai giai đoạn đầu để tự cải thiện và săn tìm các sự kiện độc hại tiếp theo.

1.  **Preparation (Chuẩn bị)**.
2.  **Detection and Analysis (Phát hiện và Phân tích)**.
3.  **Containment, Eradication, and Recovery (Ngăn chặn, Diệt trừ và Phục hồi)**.
4.  **Post-Incident Activity (Hoạt động sau sự cố)**.

> **Lưu ý:** Ngay cả khi đang phản ứng với một sự kiện, vẫn phải luôn duy trì nguồn lực ở giai đoạn *Chuẩn bị* và *Phát hiện* để đảm bảo khả năng giám sát không bị gián đoạn.

---

## 3. Các hoạt động cốt lõi

Xử lý sự cố bao gồm hai hoạt động chính là **Điều tra** và **Phục hồi**.

### A. Investigating (Điều tra)
Mục tiêu của điều tra là:
* **Tìm "Patient Zero":** Khám phá nạn nhân đầu tiên và tạo dòng thời gian sự cố (timeline).
* **Xác định Công cụ:** Tìm hiểu malware và công cụ mà kẻ địch sử dụng.
* **Tài liệu hóa (Documentation):** Ghi lại các hệ thống bị xâm nhập và hành động của kẻ tấn công.

### B. Recovering (Phục hồi)
* Tạo và triển khai kế hoạch phục hồi.
* Đưa doanh nghiệp trở lại hoạt động bình thường nếu sự cố gây gián đoạn.

---

## 4. Nguyên tắc "Vàng": Không đốt cháy giai đoạn

Điều quan trọng là đảm bảo không bỏ qua các bước trong quy trình và hoàn thành một bước trước khi chuyển sang bước tiếp theo.

> **Ví dụ cảnh báo:**
> Nếu phát hiện **10 máy bị nhiễm**, tuyệt đối **không** được tiến hành ngăn chặn chỉ 5 máy và bắt đầu diệt trừ trong khi 5 máy còn lại vẫn ở trạng thái bị nhiễm.
>
> **Hậu quả:** Hành động này sẽ báo động cho kẻ tấn công biết chúng đã bị phát hiện và đang bị truy lùng, dẫn đến những hậu quả khó lường.

---

## 5. Kết thúc sự cố (Post-Incident)

Khi một sự cố đã được xử lý hoàn toàn:
* **Báo cáo:** Phát hành báo cáo chi tiết về nguyên nhân và chi phí của sự cố.
* **Lessons Learned (Bài học kinh nghiệm):** Thực hiện các hoạt động để hiểu tổ chức cần làm gì nhằm ngăn chặn các sự cố tương tự tái diễn trong tương lai.