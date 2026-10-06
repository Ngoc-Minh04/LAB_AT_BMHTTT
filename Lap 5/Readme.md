# LAB 5: CẤU HÌNH VÀ QUẢN LÝ TƯỜNG LỬA PFSENSE

### 1\. Thông tin sinh viên

* **Họ và tên:** Nguyễn Ngọc Minh
* **MSSV:** 1150070029
* **Lớp:** 11\_TMĐT
* **Link Video YouTube:** https://youtu.be/C5yYfTnPpM0

\---

### 2\. Môi trường thực hành

* **Hệ thống Tường lửa (Firewall):** pfSense (Giao diện quản trị web: `https://10.0.0.1`)\[cite: 22].
* **Máy kiểm tra (Client/Host):** Windows 10/11 trên VMware Workstation\[cite: 22].
* **Công cụ sử dụng:** Trình duyệt web (cấu hình Firewall Rules), Command Prompt (`nslookup`, `ping`).

\---

### 3\. Nội dung các bước đã thực hiện

1. **Truy cập giao diện quản trị pfSense:**

   * Đăng nhập vào trang quản trị tường lửa qua địa chỉ `https://10.0.0.1` trên trình duyệt\[cite: 22].
2. **Cấu hình Firewall Rules (LAN Interface):**

   * Thiết lập quy tắc lọc cho giao diện LAN (`firewall\_rules\_edit.php?if=lan`)\[cite: 22].
   * Cấu hình thông số **Source** (Nguồn): Chọn `LAN subnets`\[cite: 22].
   * Cấu hình thông số **Destination** (Đích):

     * Cấu hình lọc dịch vụ phân giải tên miền **DNS (Port 53)** để kiểm soát truy vấn phân giải IP\[cite: 22].
     * Cấu hình chặn/lọc các cổng dịch vụ web tiêu chuẩn **HTTP (Port 80)** và **HTTPS (Port 445/443)**\[cite: 22].
   * Tùy chọn nâng cao (**Extra Options**): Thiết lập ghi log hệ thống (`Log packets that are handled by this rule`), thêm mô tả quản trị (**Description**) và lưu quy tắc (**Save**)\[cite: 22].
3. **Kiểm tra và kiểm chứng kết quả trên máy khách (Windows):**

   * Mở Command Prompt (`cmd`) trên máy Windows Client\[cite: 22].
   * Thực hiện kiểm tra phân giải tên miền bằng lệnh `nslookup` (ví dụ: `nslookup example.com 8.8.8.8`)\[cite: 22].
   * Quan sát trạng thái phản hồi từ tường lửa (ghi nhận thông báo `DNS request timed out`, `No response from server`) để xác minh quy tắc chặn/lọc gói tin hoạt động chính xác\[cite: 22].

\---

### 4\. Kết quả đạt được

* **Kiểm soát truy cập:** Thiết lập thành công các quy tắc lọc gói tin trên cổng LAN của tường lửa pfSense đối với các dịch vụ quan trọng (DNS, HTTP/HTTPS)\[cite: 22].
* **Xác thực cấu hình:** Qua công cụ `nslookup` trên Windows, kiểm chứng được tường lửa đã thực hiện đúng chính sách chặn/lọc (hiện tượng timeout khi truy vấn dịch vụ mạng)\[cite: 22].

