# Giai đoạn Phát hiện & Phân tích 

Ở giai đoạn này, chúng ta giả định rằng các quy trình và thủ tục đã được tạo lập, và đã có các hướng dẫn về cách hành động khi xảy ra sự cố bảo mật (Giai đoạn Chuẩn bị).

Giai đoạn **Phát hiện & Phân tích (Detection & Analysis)** bao gồm tất cả các khía cạnh của việc phát hiện sự cố, chẳng hạn như sử dụng cảm biến, nhật ký (logs) và nhân sự đã qua đào tạo. Nó cũng bao gồm việc chia sẻ thông tin, kiến thức cũng như sử dụng thông tin tình báo mối đe dọa dựa trên ngữ cảnh (context-based threat intelligence).

Việc phân đoạn kiến trúc và có sự hiểu biết rõ ràng cũng như khả năng quan sát trong mạng là những yếu tố quan trọng.

---

## 1. Các Nguồn Phát hiện (Detection Sources)

Các mối đe dọa xâm nhập vào tổ chức thông qua vô số vectơ tấn công, và việc phát hiện chúng có thể đến từ các nguồn như:

* **Nhân viên:** Người nhận thấy hành vi bất thường.
* **Cảnh báo từ công cụ:** EDR, IDS, Firewall, SIEM, v.v.
* **Hoạt động săn tìm mối đe dọa (Threat hunting).**
* **Thông báo từ bên thứ ba:** Thông báo rằng họ phát hiện dấu hiệu tổ chức của chúng ta bị xâm phạm.

## 2. Các Cấp độ Phát hiện (Levels of Detection)

Rất khuyến khích việc tạo ra các cấp độ phát hiện bằng cách phân loại mạng một cách logic như sau:

1.  **Phát hiện tại vành đai mạng (Network Perimeter):** Sử dụng tường lửa, hệ thống phát hiện/ngăn chặn xâm nhập mạng hướng internet, vùng DMZ, v.v.
2.  **Phát hiện tại cấp độ mạng nội bộ (Internal Network):** Sử dụng tường lửa cục bộ, hệ thống phát hiện/ngăn chặn xâm nhập máy chủ (HIDS), v.v.
3.  **Phát hiện tại cấp độ điểm cuối (Endpoint):** Sử dụng phần mềm diệt virus, hệ thống phát hiện & phản hồi điểm cuối (EDR), v.v.
4.  **Phát hiện tại cấp độ ứng dụng (Application):** Sử dụng nhật ký ứng dụng, nhật ký dịch vụ, v.v.

---

## 3. Điều tra Ban đầu (Initial Investigation)

Khi một sự cố bảo mật được phát hiện, cần tiến hành điều tra ban đầu và **thiết lập ngữ cảnh** trước khi tập hợp toàn bộ nhóm và gọi phản ứng sự cố toàn tổ chức.

> **Ví dụ về tầm quan trọng của ngữ cảnh:**
> Trong trường hợp một tài khoản quản trị kết nối đến một địa chỉ IP lúc `HH:MM:SS`. Nếu không biết hệ thống nào nằm ở địa chỉ IP đó và thời gian đó thuộc múi giờ nào, chúng ta rất dễ kết luận sai về sự kiện.

### Danh sách thông tin cần thu thập:
Mục tiêu là thu thập càng nhiều thông tin càng tốt về:

* **Thời gian & Người báo cáo:** Ngày/Giờ sự cố được báo cáo. Ai phát hiện và/hoặc ai báo cáo sự cố?
* **Phương thức phát hiện:** Sự cố được phát hiện như thế nào?
* **Loại sự cố:** Đó là gì? Lừa đảo (Phishing)? Hệ thống không khả dụng? v.v.
* **Hệ thống bị ảnh hưởng:** Lập danh sách các hệ thống liên quan (nếu phù hợp).
* **Nhật ký truy cập & Hành động:** Ghi lại ai đã truy cập hệ thống bị ảnh hưởng và những hành động nào đã được thực hiện.
* **Trạng thái hiện tại:** Ghi chú xem đây là sự cố đang diễn ra hay hoạt động đáng ngờ đã bị chặn đứng.
* **Chi tiết hệ thống:** Vị trí vật lý, hệ điều hành, địa chỉ IP và tên máy (hostname), chủ sở hữu hệ thống, mục đích của hệ thống, trạng thái hiện tại của hệ thống.
* **Thông tin Malware (nếu có):** Danh sách địa chỉ IP, thời gian và ngày phát hiện, loại phần mềm độc hại, hệ thống bị ảnh hưởng, xuất các tệp độc hại cùng thông tin pháp y (hashes, bản sao tệp, v.v.).

Dựa trên thông tin thu thập được, chúng ta có thể đưa ra quyết định. *Ví dụ: Chúng ta sẽ hành động khác nhau nếu biết laptop của CEO bị xâm phạm so với laptop của thực tập sinh.*

---

## 4. Dòng thời gian sự cố (Incident Timeline)

Dòng thời gian giúp giữ tổ chức trong suốt sự kiện và cung cấp bức tranh toàn cảnh. Các sự kiện được sắp xếp dựa trên thời điểm chúng xảy ra.

* **Lưu ý:** Trong quá trình điều tra, bằng chứng không nhất thiết được tìm thấy theo thứ tự thời gian. Tuy nhiên, khi sắp xếp bằng chứng theo thời gian xảy ra, chúng ta sẽ có được ngữ cảnh từ các sự kiện riêng lẻ.
* Timeline cũng giúp xác định xem bằng chứng mới phát hiện có thuộc về sự cố hiện tại hay không (Ví dụ: payload tấn công được phát hiện thực tế đã có từ 2 tuần trước trên thiết bị khác).

### Cấu trúc bảng Dòng thời gian mẫu:

| DateTime of the event | Hostname | Event description | Data source |
| :--- | :--- | :--- | :--- |
| 09/09/2021 13:31 CET | SQLServer01 | Hacker tool 'Mimikatz' was detected | Antivirus Software |

Timeline tập trung chủ yếu vào hành vi của kẻ tấn công (thời điểm tấn công, thiết lập kết nối mạng, tải tệp, v.v.). Điều quan trọng là phải ghi lại nơi hoạt động được phát hiện và các hệ thống liên quan.

---

## 5. Nền tảng quản lý vụ việc (TheHive)

Chúng ta có thể xem cảnh báo liên quan đến nhật ký sự kiện này trong Nền tảng quản lý vụ việc **TheHive**.

**Hướng dẫn thực hành:**
1.  Điều hướng đến cuối phần này và nhấp vào "Click here to spawn the target system!".
2.  Mở trang web TheHive tại `Target IP:9000` trên cổng 9000 sử dụng thông tin đăng nhập được cung cấp để xem cảnh báo.
3.  **Quy trình:** Gán cảnh báo cho bản thân -> Tạo vụ việc (case) -> Làm việc trên đó -> Thêm chi tiết về sự cố -> Tài liệu hóa tất cả phát hiện và bài học -> Đóng vụ việc.

---

## 6. Đánh giá mức độ nghiêm trọng & Phạm vi

Khi xử lý sự cố, hãy trả lời các câu hỏi sau để đánh giá mức độ nghiêm trọng:

* Tác động của việc khai thác là gì?
* Các yêu cầu để khai thác là gì?
* Có hệ thống quan trọng nào của doanh nghiệp bị ảnh hưởng không?
* Có các bước khắc phục được đề xuất không?
* Bao nhiêu hệ thống đã bị ảnh hưởng?
* Mã khai thác (exploit) có đang được sử dụng rộng rãi trong thực tế (in the wild) không?
* Mã khai thác có khả năng tự lây lan như sâu (worm-like) không?

> Hai câu hỏi cuối có thể chỉ ra mức độ tinh vi của đối thủ. Các sự cố có tác động cao sẽ được xử lý ngay lập tức, và sự cố có số lượng hệ thống bị ảnh hưởng lớn sẽ phải được leo thang.

---

## 7. Tính bảo mật & Truyền thông

Sự cố là chủ đề rất bí mật.

* **Nguyên tắc:** Tất cả thông tin thu thập được nên giữ trên cơ sở **"Cần-phải-biết" (Need-to-know)** trừ khi luật pháp hoặc quyết định quản lý yêu cầu khác.
* **Lý do:** Đối thủ có thể là nhân viên của công ty (Insider threat).
* **Truyền thông:** Nếu xảy ra vi phạm dữ liệu, việc liên lạc với các bên nội bộ và bên ngoài phải do người được chỉ định thực hiện theo quy định của bộ phận pháp lý.

**Thiết lập kỳ vọng:**
Khi mở cuộc điều tra, cần xác định:
1.  Loại sự cố đã xảy ra.
2.  Nguồn bằng chứng hiện có.
3.  Ước tính sơ bộ về thời gian cần thiết cho cuộc điều tra.
4.  Kỳ vọng về việc có thể xác định danh tính đối thủ hay không.

*Điều quan trọng là giữ cho mọi người liên quan và ban quản lý được thông báo về bất kỳ tiến triển và kỳ vọng nào.*

---




# Giai đoạn Phát hiện & Phân tích (Phần 2)

Mục tiêu cốt lõi của việc điều tra là hiểu rõ **điều gì đã xảy ra** và **nó xảy ra như thế nào**. 

> **Tại sao phải tìm hiểu cách thức tấn công?**
> Nếu chỉ đơn giản là cài đặt lại hệ thống mà không biết lỗ hổng nằm ở đâu, kẻ tấn công có thể dễ dàng lặp lại các bước tương tự để tái xâm nhập. Việc hiểu rõ công cụ và đường đi của đối thủ giúp chúng ta lập kế hoạch khắc phục triệt để.

---

## 1. Quy trình Điều tra Chu kỳ (3 Bước)

Cuộc điều tra là một quá trình lặp đi lặp lại gồm 3 bước chính:

1.  **Tạo và sử dụng các Chỉ số xâm phạm (IOCs):** Tài liệu hóa các dấu vết kỹ thuật.
2.  **Xác định manh mối mới và hệ thống bị ảnh hưởng:** Tìm kiếm dấu hiệu xâm phạm trên toàn mạng.
3.  **Thu thập và phân tích dữ liệu:** Thu thập bằng chứng từ các hệ thống mới phát hiện để tìm thêm manh mối.



---

## 2. Chỉ số xâm phạm (Indicator of Compromise - IOC)

IOC là những dấu hiệu cho thấy một sự cố đã xảy ra (ví dụ: địa chỉ IP, mã băm tệp, tên tệp lạ).

### Các tiêu chuẩn phổ biến:
* **OpenIOC & YARA:** Ngôn ngữ tiêu chuẩn để mô tả các tạo tác (artifacts).
* **STIX (Structured Threat Information eXpression):** Định dạng dựa trên JSON dùng để chia sẻ thông tin tình báo mối đe dọa.

**Ví dụ về dữ liệu IOC định dạng STIX:**
```json
{
    "type": "file",
    "id": "file--474454e8-d393-5a4f-9069-19631ea9d397",
    "hashes": {
        "MD5": "40e609840ef3f7fea94d53998ec9f97f",
        "SHA-256": "3461da3a2ddcced4a00f87dcd7650af48f97998a3ac9ca649d7ef3b7332bd997"
    },
    "name": "osvmhdfl.dll"
}
```

### 3. Lưu ý Quan trọng về An toàn Thông tin xác thực

Trong quá trình điều tra, việc kết nối vào các máy bị nhiễm có thể khiến thông tin xác thực (**credentials**) của quản trị viên bị lộ nếu không cẩn thận.

* **Nên dùng:** Giao thức **WinRM** hoặc các công cụ sử dụng **Network Logon (Logon Type 3)** vì chúng thường không lưu mật khẩu (cache) trên hệ thống từ xa.
* **⚠️ Cảnh báo về PsExec:** Nếu dùng `PsExec` với thông tin xác thực rõ ràng (*explicit credentials*), mật khẩu **SẼ** bị lưu trên máy bị nhiễm, tạo cơ hội cho kẻ tấn công đánh cắp thông qua các kỹ thuật như *Credential Dumping*.

---

### 4. Thu thập & Phân tích Dữ liệu

Khi xác định được hệ thống bị ảnh hưởng, chúng ta cần thu thập dữ liệu để phân tích nhằm trả lời các câu hỏi về sự cố:

* **Live Response (Phản ứng trực tiếp):** Thu thập dữ liệu ngay khi máy đang chạy. Điều này cực kỳ quan trọng vì nhiều dấu vết (như tiến trình mã độc, kết nối mạng) chỉ tồn tại trong **RAM** và sẽ mất hoàn toàn nếu máy bị tắt.
* **Pháp y kỹ thuật số (Forensics):** * **Malware Analysis:** Phân tích hành vi và đặc điểm của mã độc.
    * **Disk Forensics:** Kiểm tra dữ liệu trên ổ cứng để tìm dấu vết lịch sử.
* **Chain of Custody (Chuỗi hành trình chứng cứ):** Phải ghi chép và bảo quản dữ liệu nghiêm ngặt để đảm bảo bằng chứng có giá trị pháp lý nếu tổ chức quyết định thực hiện các hành động pháp lý đối với kẻ tấn công.



---

### 5. Ứng dụng AI trong Phát hiện Mối đe dọa

AI đang thay đổi cách các tổ chức phát hiện và phản hồi sự cố bằng cách tự động hóa các tác vụ tốn thời gian:

* **Tóm tắt câu chuyện tấn công (Attack Story):** AI (ví dụ: *Elastic Security*) có thể phân tích hàng ngàn cảnh báo để nhóm chúng lại thành một chuỗi sự kiện logic, giúp điều tra viên nắm bắt toàn cảnh nhanh chóng.
* **Xác định hành vi bất thường:** AI học từ dữ liệu lịch sử để phát hiện các dấu hiệu tấn công tinh vi mà các quy tắc tĩnh (static rules) thường bỏ sót.
* **Tự động phân loại (Triage):** Tự động đánh giá mức độ nghiêm trọng để ưu tiên xử lý các cảnh báo quan trọng nhất trước.



---

### 6. Thực hành trên Case Management (TheHive)

Để quản lý cuộc điều tra một cách chuyên nghiệp và có tổ chức, chúng ta sử dụng nền tảng **TheHive**:

1.  **Truy cập:** Mở trình duyệt và đi đến `Target IP:9000`.
2.  **Tiếp nhận:** Xem danh sách cảnh báo (Alerts), tự gán (**Assign**) cảnh báo cho bản thân.
3.  **Khởi tạo:** Chuyển đổi cảnh báo thành một vụ việc (**Case**) để bắt đầu điều tra chính thức.
4.  **Phân tích:** Thêm các chỉ số quan sát được (**Observables**) như địa chỉ IP, mã băm tệp (Hash), hoặc URL độc hại vào vụ việc.
5.  **Hoàn tất:** Ghi lại mọi phát hiện, bài học kinh nghiệm và đóng vụ việc sau khi đã xử lý triệt để.