# Hướng dẫn Tạo và Tinh chỉnh Trực quan hóa SIEM: Các lần đăng nhập thất bại

Quy trình này hướng dẫn cách xây dựng một Dashboard từ đầu trên Kibana để hiển thị các lần đăng nhập thất bại, sau đó tinh chỉnh nó để dữ liệu rõ ràng và chính xác hơn.

## 1. Khởi tạo Dashboard và Visualization đầu tiên
Để bắt đầu, chúng ta cần tạo một không gian chứa (Dashboard) và thiết lập các thông số cơ bản.

* **Tạo Dashboard:** Truy cập vào menu "Dashboard" trên thanh điều hướng, xóa Dashboard cũ (nếu có) và chọn "Create new dashboard".
* **Tạo Visualization:** Nhấn nút "Create visualization" để mở giao diện thiết lập.
* **Thiết lập thời gian:** Trước khi cấu hình, cần mở bộ chọn thời gian (time picker) và đặt phạm vi là "last 15 years", sau đó nhấn "Apply".

## 2. Cấu hình Visualization (Dạng Bảng)
Tại cửa sổ thiết lập Visualization, có 4 yếu tố chính cần lưu ý và cấu hình:

1.  **Bộ lọc dữ liệu (Filter):** Thiết lập bộ lọc để chỉ lấy các sự kiện có `event.code` là `4625` (Mã sự kiện cho đăng nhập thất bại trên Windows).
2.  **Chọn tập dữ liệu (Index):** Chọn `windows*` trong phần "Index pattern" để sử dụng dữ liệu từ các nguồn Windows.
3.  **Kiểm tra trường dữ liệu:** Sử dụng thanh tìm kiếm để đảm bảo trường dữ liệu cần thiết (ví dụ: `user.name.keyword`) có tồn tại.
    * *Lưu ý:* Sử dụng đuôi `.keyword` (như `user.name.keyword`) là cần thiết khi thực hiện các phép tổng hợp (aggregations).
4.  **Chọn loại biểu đồ:** Chọn loại "Table" (Bảng) từ menu thả xuống.

**Cấu hình chi tiết cho Bảng:**
* **Rows (Hàng):**
    * Thêm trường `user.name.keyword` để hiển thị tên người dùng.
    * Thêm trường `host.hostname.keyword` để hiển thị tên máy tính nơi xảy ra sự cố.
* **Metrics (Chỉ số):** Chọn "Count" để đếm số lần xuất hiện của sự kiện.
* **Lưu:** Nhấn "Save and return" và sau đó lưu Dashboard.



## 3. Tinh chỉnh Visualization (Refining)
Dựa trên yêu cầu quản lý SOC, Visualization cần được tinh chỉnh để rõ ràng hơn và loại bỏ dữ liệu rác.

### Các bước chỉnh sửa:
Truy cập Dashboard, chọn biểu tượng chỉnh sửa (hình bút chì), sau đó nhấn vào biểu tượng bánh răng (gear) trên bảng dữ liệu và chọn "Edit lens".

### Nội dung tinh chỉnh:
1.  **Đổi tên cột:** Thay đổi các nhãn mặc định (như "Top values of...") thành tên dễ hiểu hơn cho các cột User và Hostname.
2.  **Thêm thông tin:** Thêm cột "Logon Type" bằng cách sử dụng trường `winlog.logon.type.keyword`.
3.  **Sắp xếp (Sorting):** Cấu hình tính năng sắp xếp cho bảng kết quả.
4.  **Lọc bỏ người dùng cụ thể:** Thêm bộ lọc để loại trừ các username không cần giám sát (ví dụ: DESKTOP-DPOESND, WIN-OK9BH1BCKSD...).
5.  **Lọc bỏ tài khoản máy tính:** Sử dụng truy vấn KQL để loại bỏ các tài khoản máy tính (thường kết thúc bằng dấu `$`) và đảm bảo chỉ lấy log bảo mật.
    * *Truy vấn KQL:* `NOT user.name: *$ AND winlog.channel.keyword: Security`.

### Hoàn tất:
* Đặt tiêu đề cho Visualization.
* Nhấn "Save and return" và lưu lại Dashboard.