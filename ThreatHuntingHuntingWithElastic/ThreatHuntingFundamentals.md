# Tóm tắt: Nền tảng Săn tìm Mối đe dọa (Threat Hunting Fundamentals)

## 1. Định nghĩa và Mục tiêu
* **Định nghĩa:** Là một hoạt động chủ động (proactive), do con người dẫn dắt và thường dựa trên giả thuyết (hypothesis-driven).
* **Mục tiêu chính:** Giảm thiểu **"Dwell Time"** (thời gian lưu trú của kẻ tấn công) bằng cách tìm kiếm các mối đe dọa ẩn mình mà các giải pháp bảo mật tự động bỏ sót.
* **Cách tiếp cận:** Chuyển dịch từ phòng ngự thụ động sang chiến lược tấn công chủ động để phát hiện kẻ địch ở giai đoạn sớm nhất trong **Cyber Kill Chain**.

## 2. Các khía cạnh then chốt
| Khía cạnh | Mô tả |
| :--- | :--- |
| **Săn tìm (Hunting)** | Chiến lược tấn công chủ động dựa trên giả thuyết và TTPs (Tactics, Techniques, and Procedures). |
| **Phản ứng (Incident Response)** | Chiến lược tấn công thụ động, tìm kiếm dấu vết dựa trên một sự cố đã được xác thực. |
| **Kỹ năng cần thiết** | Hiểu biết về TTPs, Cyber Kill Chain, tư duy của kẻ tấn công (Adversarial Mindset). |
| **Dữ liệu & Phân tích** | Sử dụng dữ liệu độ tin cậy cao, phân tích hành vi và các công cụ/nền tảng tiên tiến. |

## 3. Mối quan hệ với Xử lý sự cố (Incident Handling)
Threat Hunting đan xen vào các giai đoạn của Incident Handling:
* **Chuẩn bị (Preparation):** Thiết lập quy tắc tham chiến (Rules of Engagement) và giao thức vận hành.
* **Phát hiện & Phân tích (Detection & Analysis):** Thợ săn giúp xác thực các chỉ số thỏa hiệp (IoCs) và tìm thêm các dấu vết bị bỏ lỡ.
* **Ngăn chặn, Loại bỏ & Phục hồi:** Hỗ trợ điều tra sâu trong khi đội IR xử lý sự cố.
* **Hoạt động sau sự cố (Post-Incident):** Đưa ra khuyến nghị củng cố hệ thống dựa trên kiến thức chuyên sâu.

## 4. Cấu trúc đội ngũ Săn tìm mối đe dọa
Một đội ngũ lý tưởng cần sự kết hợp đa dạng các vai trò:
1.  **Threat Hunter:** Vai trò cốt lõi, tìm kiếm IoCs và thực hiện giả thuyết.
2.  **Threat Intelligence Analyst:** Thu thập và phân tích dữ liệu từ OSINT, Dark Web, feeds.
3.  **Incident Responders:** Quản lý tình huống, ngăn chặn và phục hồi.
4.  **Forensics Experts:** Điều tra kỹ thuật số, phân tích mã độc và kỹ thuật đảo ngược.
5.  **Data Analysts/Scientists:** Sử dụng ML/AI để tìm ra các mẫu (patterns) trong tập dữ liệu lớn.
6.  **Security Engineers/Architects:** Thiết kế hạ tầng bảo mật và triển khai công cụ săn tìm.
7.  **Network Security Analyst:** Chuyên gia về lưu lượng và hành vi mạng.
8.  **SOC Manager:** Điều phối và quản lý vận hành chung.

## 5. Khi nào cần thực hiện Săn tìm (Trình kích hoạt)
Việc săn tìm nên diễn ra liên tục, nhưng đặc biệt khẩn cấp khi:
* **Thông tin mới về đối thủ/lỗ hổng:** Có thông tin về một lỗ hổng Zero-day hoặc đối thủ mới.
* **Chỉ số mới (New IoCs):** Xuất hiện các IoCs mới liên quan đến các nhóm tin tặc từng tấn công tổ chức hoặc lĩnh vực tương tự.
* **Bất thường mạng (Anomalies):** Phát hiện nhiều hoạt động lạ xuất hiện cùng lúc.
* **Trong quá trình IR:** Săn tìm song song để đảm bảo không còn mối đe dọa nào khác ẩn nấp.

## 6. Mối quan hệ với Đánh giá rủi ro (Risk Assessment)
Đánh giá rủi ro là nền tảng giúp Threat Hunting hiệu quả hơn thông qua:
* **Ưu tiên nguồn lực:** Tập trung vào các tài sản quan trọng nhất (**Crown Jewels**).
* **Xây dựng giả thuyết:** Hiểu lỗ hổng hiện có để dự đoán cách kẻ tấn công sẽ khai thác.
* **Tối ưu hóa Threat Intel:** Áp dụng trí tuệ mối đe dọa vào đúng ngữ cảnh của tổ chức.
* **Cải thiện kiểm soát:** Kết quả từ săn tìm quay lại bổ sung cho các chiến lược giảm thiểu rủi ro.