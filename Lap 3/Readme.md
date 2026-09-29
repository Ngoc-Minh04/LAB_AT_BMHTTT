# LAB 4: KHẢO SÁT VÀ ĐÁNH GIÁ BỀ MẶT MẠNG BẰNG NMAP

### 1. Thông tin sinh viên

* **Họ và tên:** Nguyễn Ngọc Minh
* **MSSV:** 1150070029
* **Lớp:** 11_TMĐT
* **Link Video YouTube:** https://www.youtube.com/watch?v=xQgcb4AFs34

---

### 2. Môi trường thực hành

* **Máy quét chính (Attacker/Client - VM 1):** Kali Linux (`192.168.56.10`), kết nối qua VirtualBox Host-Only Network[cite: 1].
* **Máy mục tiêu (Target - VM 2):** Metasploitable 2 (`192.168.56.101`) - Máy ảo cố ý có lỗ hổng[cite: 1].
* **Máy thật (Host):** Windows 10 / Windows 11 (`192.168.56.1`)[cite: 1].

---

### 3. Nội dung đã thực hiện

1. Chuẩn bị môi trường mạng Host-Only, kiểm tra kết nối ping từ Kali sang Metasploitable 2[cite: 1].
2. Cài đặt và kiểm tra phiên bản Nmap trên Kali Linux[cite: 1].
3. Thực hiện phát hiện host đang hoạt động trong mạng (`-sn`)[cite: 1].
4. Khảo sát cổng TCP và so sánh các kỹ thuật quét:
   * TCP Connect Scan (`-sT`)[cite: 1]
   * SYN Scan (`-sS`)[cite: 1]
   * FIN, Xmas, NULL Scan (`-sF`, `-sX`, `-sN`)[cite: 1]
   * ACK Scan (`-sA`) quan sát chính sách lọc[cite: 1]
5. Thực hiện quét cổng UDP có kiểm soát (`-sU`)[cite: 1].
6. Nhận diện phiên bản dịch vụ (`-sV`) và hệ điều hành (`-O`), chạy quét tổng hợp (`-A`)[cite: 1].
7. Sử dụng Nmap Scripting Engine (NSE) để thu thập thông tin SMB (`smb-os-discovery`) và kiểm tra lỗ hổng MS17-010 (`smb-vuln-ms17-010`)[cite: 1].
8. Xuất kết quả ra các định dạng (Normal text, XML, Grepable) và chuyển đổi sang HTML bằng `xsltproc`[cite: 1].

---

### 4. Kết quả đạt được

* **Phát hiện Host & Port:** Xác định chính xác các dịch vụ đang mở trên Metasploitable 2 (FTP, SSH, HTTP, SMB, MySQL...) cùng trạng thái cổng (`open`, `closed`, `filtered`).
* **Đánh giá lỗ hổng:** Nhận diện được các dịch vụ phiên bản cũ và kiểm chứng dấu hiệu lỗ hổng nghiêm trọng (như MS17-010) qua NSE script[cite: 1].
* **Báo cáo kỹ thuật:** Tạo lập thành công bộ hồ sơ bằng chứng gồm các file kết quả (`.txt`, `.xml`, `.html`) phục vụ công tác kiểm tra an toàn hệ thống[cite: 1].

---

### 5. Lệnh kiểm tra mẫu trên Kali Linux

```bash
# Kiểm tra phiên bản Nmap
nmap --version

# Quét SYN kết hợp nhận diện dịch vụ và hệ điều hành, xuất báo cáo
sudo nmap -sS -sV -O 192.168.56.101 -oN ket_qua.txt