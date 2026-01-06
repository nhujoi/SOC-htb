# Kiến thức An Ninh Mạng: Cyber Kill Chain & MITRE ATT&CK

Tài liệu này tổng hợp các khái niệm cốt lõi về vòng đời tấn công mạng, khung phân tích hành vi kẻ tấn công và cách áp dụng trong quản lý sự cố.

---

## 1. Cyber Kill Chain (Chuỗi Tiêu Diệt Mạng)

**Cyber Kill Chain** là mô hình mô tả vòng đời của một cuộc tấn công mạng, gồm 7 giai đoạn. Hiểu mô hình này giúp xác định kẻ tấn công đang ở đâu và ngăn chặn chúng tiến sâu hơn vào mạng lưới.
![alt text](/Incident_Handling_Process/image/image.png)
### Các giai đoạn của Cyber Kill Chain:

1.  **Reconnaissance (Trinh sát):**
    * Kẻ tấn công chọn mục tiêu và thu thập thông tin.
    * **Thụ động:** Tìm kiếm qua LinkedIn, Instagram, website công ty (đối tác, thông tin tuyển dụng để lộ công nghệ sử dụng).
    * **Chủ động:** Quét (scan) các ứng dụng web và địa chỉ IP của tổ chức.

2.  **Weaponize (Vũ khí hóa):**
    * Tạo ra mã độc (malware) hoặc exploit dựa trên thông tin đã trinh sát.
    * Mã độc được thiết kế nhẹ, khó bị phát hiện bởi antivirus/EDR.

3.  **Delivery (Phân phối):**
    * Gửi "vũ khí" đến nạn nhân.
    * **Phương thức:** Phishing email (đính kèm file/link), website giả mạo, hoặc qua thiết bị vật lý (USB).

4.  **Exploitation (Khai thác):**
    * Kích hoạt mã độc trên máy nạn nhân.
    * Mục tiêu là thực thi mã lệnh (code execution) để giành quyền truy cập.

5.  **Installation (Cài đặt):**
    * Cài đặt công cụ để duy trì quyền kiểm soát (persistence).
    * **Kỹ thuật phổ biến:**
        * *Droppers:* Mã nhỏ để tải malware chính.
        * *Backdoors:* Cửa sau để truy cập lại.
        * *Rootkits:* Mã độc ẩn mình sâu trong hệ thống để tránh bị phát hiện.

6.  **Command and Control (C2 - Ra lệnh và Kiểm soát):**
    * Thiết lập kênh liên lạc từ xa giữa máy bị xâm nhập và máy chủ của kẻ tấn công.

7.  **Action on Objectives (Hành động theo mục tiêu):**
    * Thực hiện mục đích cuối cùng: Trích xuất dữ liệu, cài Ransomware, phá hoại hệ thống.

> **Lưu ý:** Kẻ tấn công không hoạt động tuyến tính. Sau khi xâm nhập (Installation), chúng thường quay lại bước **Recon** bên trong mạng nội bộ để tìm mục tiêu mới.

---

## 2. MITRE ATT&CK Framework

**MITRE ATT&CK** là cơ sở kiến thức dạng ma trận về chiến thuật và kỹ thuật của kẻ tấn công, chi tiết hơn Cyber Kill Chain.

* **Website:** [https://attack.mitre.org/matrices/enterprise/](https://attack.mitre.org/matrices/enterprise/)

### Cấu trúc chính:

* **Tactic (Chiến thuật):** Mục tiêu cấp cao của kẻ tấn công (Họ muốn đạt được điều gì?).
    * *Ví dụ:* Initial Access, Persistence, Privilege Escalation.
* **Technique (Kỹ thuật):** Cách thức cụ thể để đạt được mục tiêu đó.
    * *Ví dụ:* **T1105 (Ingress Tool Transfer)** - Dùng `wget`, `curl` để tải công cụ.
    * *Ví dụ:* **T1021 (Remote Services)** - Dùng SSH, RDP, SMB.
* **Sub-technique (Kỹ thuật con):** Chi tiết cụ thể hơn của kỹ thuật.
    * *Ví dụ:* **T1003.001 (OS Credentials: LSASS Memory)** - Dump mật khẩu từ bộ nhớ LSASS.
![alt text](/Incident_Handling_Process/image/image-1.png)
---

## 3. Pyramid of Pain (Kim tự tháp Đau thương)

Biểu đồ minh họa mức độ khó khăn gây ra cho kẻ tấn công khi các chỉ số (indicators) bị phát hiện và ngăn chặn.
![alt text](/Incident_Handling_Process/image/image-2.png)
| Mức độ | Loại Chỉ số | Mức độ "Đau" cho Hacker | Giải thích |
| :--- | :--- | :--- | :--- |
| **Đáy** | Hash Values, IP Addresses | **Thấp (Trivial/Easy)** | Dễ dàng thay đổi. Chặn IP chỉ làm chậm hacker một chút. |
| **Giữa** | Network/Host Artifacts | **Trung bình (Annoying)** | Các dấu vết như registry key, tên file. Khó đổi hơn một chút. |
| **Đỉnh** | **TTPs (Tactics, Techniques)** | **Cao (Tough)** | Đây là hành vi cốt lõi (liên quan đến MITRE). Nếu bị chặn, hacker phải thay đổi hoàn toàn cách thức hoạt động. |

---

## 4. Ứng dụng: TheHive & Mapping

**TheHive** là nền tảng quản lý sự cố (Incident Response Platform), cho phép tích hợp MITRE ATT&CK để ánh xạ các cảnh báo.

### Thông tin truy cập Lab (Ví dụ):
* **URL:** `http://TARGET_IP:9000`
* **Username:** `htb-analyst`
* **Password:** `P3n#31337@LOG`

### Ví dụ Mapping: Ransomware Incident

Dưới đây là bảng ánh xạ các hành vi quan sát được trong một vụ tấn công thực tế vào khung MITRE ATT&CK:

| Tactic (Chiến thuật) | Technique (Kỹ thuật) | ID | Mô tả hành vi |
| :--- | :--- | :--- | :--- |
| **Initial Access** | Exploit Public-Facing Application | `T1190` | Khai thác lỗ hổng CVE trên Confluence. |
| **Execution** | Command and Scripting Interpreter: PowerShell | `T1059.001` | Sử dụng PowerShell để tải payload. |
| **Persistence** | Windows Service | `T1543.003` | Tạo Windows Service để duy trì truy cập. |
| **Credential Access** | OS Credential Dumping: LSASS Memory | `T1003.001` | Trích xuất thông tin đăng nhập từ bộ nhớ. |
| **Lateral Movement** | Remote Services: Remote Desktop Protocol | `T1021.001` | Di chuyển ngang trong mạng qua RDP. |
| **Impact** | Data Encrypted for Impact | `T1486` | Mã hóa dữ liệu (Ransomware LockBit). |