# BÁO CÁO THỰC HÀNH LAB 7: CẤU HÌNH SQUID PROXY VÀ CHẶN WEBSITE HTTPS TRÊN PFSENSE

* **Họ và tên:** Nguyễn Ngọc Minh  
* **MSSV:** 1150070029  
* **Lớp:** 11TMĐT  
* **Môn học:** An toàn hệ thống thông tin  
* **Video hướng dẫn (YouTube):** [https://youtu.be/DjOP3VLRsjg](https://youtu.be/DjOP3VLRsjg)

---

## I. TỔNG QUAN VỀ BÀI LAB

Bài lab này hướng dẫn cấu hình dịch vụ **Squid Proxy** kết hợp với **SSL Interception (SSL Man-In-The-Middle Filtering)** trên tường lửa **pfSense (Phiên bản 2.7.2-RELEASE)** nhằm quản lý, giải mã và chặn các truy cập đến các website HTTPS nằm trong danh sách đen (**Blacklist**), cụ thể ở đây là `facebook.com`.

---

## II. CÁC BƯỚC THỰC HIỆN CHI TIẾT

### Bước 1: Đăng nhập giao diện quản trị pfSense WebGUI
1. Khởi động máy ảo **Windows 10 Client** và mở trình duyệt web (Microsoft Edge).
2. Truy cập vào địa chỉ IP của tường lửa pfSense: `https://10.0.0.1`.
3. Đăng nhập bằng tài khoản quản trị (`admin`).
4. Màn hình **Dashboard** hiển thị thông tin hệ thống pfSense hoạt động bình thường.

---

### Bước 2: Cấu hình danh sách chặn tên miền (Blacklist) trong Squid Proxy
1. Trên thanh menu chính, truy cập: **Services** $\rightarrow$ **Squid Proxy Server**.
2. Chuyển sang tab **ACLs** (Access Control Lists).
3. Kéo xuống mục **Blacklist**, nhập tên miền trang web muốn chặn:
   ```text
   facebook.com
   ```
4. Cuộn xuống cuối trang và nhấn nút **Save** để lưu thiết lập danh sách chặn.

---

### Bước 3: Tạo chứng chỉ số nội bộ (Certificate Authority - CA)
Để Squid Proxy có thể giải mã và can thiệp lọc lưu lượng mã hóa HTTPS, cần tạo một chứng chỉ CA nội bộ trên pfSense:

1. Truy cập menu: **System** $\rightarrow$ **Cert. Manager** (hoặc **Certificate** $\rightarrow$ **Authorities**).
2. Bấm nút **+ Add** để thêm chứng chỉ mới và thiết lập các thông số:
   * **Descriptive name:** `pfSense-CA`
   * **Method:** Chọn `Create an internal Certificate Authority`
   * **Common Name:** `internal-ca`
3. Nhấn **Save** để hoàn tất việc khởi tạo.
4. Kiểm tra danh sách Certificate Authorities, chứng chỉ `pfSense-CA` đã xuất hiện và ở trạng thái sẵn sàng sử dụng.

---

### Bước 4: Kích hoạt tính năng lọc HTTPS (SSL Man In The Middle Filtering)
1. Quay lại menu: **Services** $\rightarrow$ **Squid Proxy Server** $\rightarrow$ tab **General**.
2. Kéo xuống phần **SSL Man In The Middle Filtering** và cấu hình các thông số sau:
   * **HTTPS/SSL Interception:** Tích chọn `Enable SSL filtering` để kích hoạt tính năng lọc SSL/HTTPS.
   * **SSL/MITM Mode:** Chọn `Splice Whitelist, Bump Otherwise`.
   * **SSL Intercept Interface(s):** Chọn giao diện `LAN`.
   * **CA:** Chọn chứng chỉ `pfSense-CA` vừa tạo ở Bước 3.
3. Cuộn xuống cuối trang, chọn **Save** và nhấn nút **Apply Changes** ở thanh thông báo màu vàng phía trên để hệ thống áp dụng toàn bộ cấu hình mới.

---

### Bước 5: Cài đặt Chứng chỉ CA lên Máy Trạm (Windows Client)
1. Vào **System** $\rightarrow$ **Cert. Manager** $\rightarrow$ tab **Authorities**.
2. Bấm biểu tượng **Export CA** ở hàng `pfSense-CA` để tải tệp chứng chỉ về máy.
3. Mở tệp chứng chỉ vừa tải trên máy client $\rightarrow$ chọn **Install Certificate**.
4. Chọn Store Location là **Local Machine** $\rightarrow$ chọn **Place all certificates in the following store**.
5. Nhấn **Browse** $\rightarrow$ chọn **Trusted Root Certification Authorities** $\rightarrow$ nhấn **Next** và **Finish**.

---

## III. KẾT LUẬN & ĐÁNH GIÁ KẾT QUẢ

1. Sau khi hoàn tất các bước cấu hình và cài đặt chứng chỉ CA, Squid Proxy đã có thể can thiệp, giải mã và phân tích các yêu cầu gửi qua giao thức HTTPS từ mạng LAN.
2. Tất cả các yêu cầu truy cập tới các tên miền nằm trong danh sách **Blacklist** (ví dụ: `facebook.com`) sẽ lập tức bị tường lửa pfSense ngăn chặn và hiển thị trang thông báo **Access Denied**.
3. Các trang web không thuộc danh sách bị chặn vẫn truy cập bình thường, đảm bảo tính bảo mật và kiểm soát lưu lượng mạng hiệu quả.