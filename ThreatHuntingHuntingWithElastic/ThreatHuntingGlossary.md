# Thuật ngữ và Mô hình Săn tìm Mối đe dọa (Threat Hunting Glossary)

## 1. Đối tượng và Tổ chức Tấn công
* **Adversary (Đối thủ):** Thực thể chưa được cấp phép tìm cách xâm nhập tổ chức để đạt mục đích (tài chính, sở hữu trí tuệ...). Phân loại: Tội phạm mạng, mối đe dọa nội bộ, hacktivists, hoặc các nhóm do nhà nước bảo trợ.
* **APT (Advanced Persistent Threat):** Các nhóm tổ chức cao (thường là cấp quốc gia). 
    * *Advanced:* Không chỉ là công nghệ cao mà còn là khả năng lập kế hoạch tinh vi.
    * *Persistent:* Sự kiên trì bám trụ mục tiêu nhờ nguồn lực dồi dào (tiền bạc, nhân lực, thời gian).

## 2. Phương thức hoạt động (TTPs)
**TTPs** là "chữ ký" hoạt động của đối thủ, bao gồm:
* **Tactics (Chiến thuật):** Mục tiêu chiến lược (Câu hỏi: **Tại sao?**).
* **Techniques (Kỹ thuật):** Các phương pháp cụ thể để đạt mục tiêu chiến thuật (Câu hỏi: **Như thế nào?**).
* **Procedures (Quy trình):** Các bước thực hiện chi tiết, cụ thể như một "công thức nấu ăn".

## 3. Các chỉ số và Thành phần của Mối đe dọa
* **Indicator (Chỉ số):** Là sự kết hợp giữa **Dữ liệu kỹ thuật + Ngữ cảnh**. Dữ liệu thiếu ngữ cảnh sẽ không có giá trị phòng thủ.
* **Threat (Mối đe dọa):** Được hình thành từ 3 yếu tố:
    $$\text{Threat} = \text{Intent (Ý đồ)} + \text{Capability (Năng lực)} + \text{Opportunity (Cơ hội)}$$
* **IOCs (Chỉ số thỏa hiệp):** Các dấu vết kỹ thuật còn sót lại (Hash, IP, URL, Domain) của một cuộc tấn công.
* **Campaign (Chiến dịch):** Tập hợp các sự cố có chung TTPs và mục tiêu thu thập thông tin tương tự nhau.

## 4. Mô hình Kim tự tháp Nỗi đau (Pyramid of Pain)
Mô hình này thể hiện mức độ khó khăn gây ra cho kẻ tấn công khi thợ săn tìm thấy các chỉ số của chúng:

| Cấp độ | Loại chỉ số | Độ khó đối với kẻ địch | Giải thích |
| :--- | :--- | :--- | :--- |
| **Đỉnh** | **TTPs** | Rất khó (Tough) | Buộc kẻ địch phải thay đổi toàn bộ cách vận hành. |
| **Cao** | **Tools** | Thử thách (Challenging) | Buộc chúng phải tìm kiếm hoặc viết lại công cụ mới. |
| **Trung bình** | **Artifacts** | Khó chịu (Annoying) | Dấu vết mạng/máy chủ (Registry, log...). |
| **Thấp** | **Domains** | Đơn giản (Simple) | Tên miền có thể thay đổi nhanh qua DGA. |
| **Thấp** | **IP Addresses** | Dễ (Easy) | Có thể thay đổi qua VPN, Proxy. |
| **Đáy** | **Hash Values** | Tầm thường (Trivial) | Chỉ cần đổi 1 byte dữ liệu là hash thay đổi. |



## 5. Mô hình Kim cương (Diamond Model)
Dùng để phân tích mối quan hệ giữa 4 thành phần chính của một cuộc xâm nhập:
1.  **Adversary (Đối thủ):** Kẻ chịu trách nhiệm cho cuộc tấn công.
2.  **Capability (Năng lực):** Công cụ, kỹ thuật mà đối thủ sử dụng.
3.  **Infrastructure (Hạ tầng):** Tài nguyên vật lý/ảo (Server, IP, Domain) dùng để tấn công.
4.  **Victim (Nạn nhân):** Mục tiêu bị tấn công (Cá nhân, tổ chức).



*So sánh:* Trong khi **Cyber Kill Chain** tập trung vào các giai đoạn của cuộc tấn công, **Diamond Model** tập trung vào mối quan hệ và sự tương tác giữa các thực thể trong hệ sinh thái xâm nhập.