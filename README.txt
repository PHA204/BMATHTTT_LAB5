# LAB5 - pfSense: Xây dựng và Cấu hình Tường lửa LAN, DMZ

- Họ và tên: Phạm Hữu Ân
- MSSV: 1150080084
- Mã lớp: CNPM1
- Tên lab: LAB5 - pfSense

## Phiên bản môi trường

- Windows Server: Windows Server 2019 / 2022
- VMware Workstation / VirtualBox: 17.x / 7.x
- pfSense Firewall: pfSense CE (Community Edition)
- Wireshark: 4.6.8 (để kiểm tra lưu lượng mạng)

## Cách dựng môi trường

1. Tạo máy ảo pfSense Firewall với tối thiểu 3 card mạng (WAN, LAN, DMZ).
2. Tạo máy ảo Windows Server đặt tại phân vùng mạng LAN để kiểm thử.
3. Cấu hình các dải mạng ảo (VMnet / Internal Network):
   - **WAN**: Kết nối mạng ngoài/Internet.
   - **LAN**: Kết nối mạng nội bộ (`VMnet1` - dải `192.168.126.0/24`)[cite: 7, 8].
   - **DMZ**: Kết nối vùng máy chủ dịch vụ (`VMnet2` - dải `172.16.16.0/24`)[cite: 7].
4. Tiến hành cài đặt và phân bổ IP, thiết lập DHCP Server cho các giao diện pfSense.
5. Thực hiện cấu hình Firewall Rules, NAT và kiểm thử lưu lượng.

## Các tình huống đã thực hiện

### TH1 - Cài đặt và Cấu hình Giao diện pfSense
- Gán card mạng cho WAN, LAN và DMZ trên pfSense.
- Cấu hình địa chỉ IP tĩnh cho mạng LAN và DMZ.
- Truy cập thành công trang quản trị web của pfSense từ Windows Server.
- Kết quả: PASS.

### TH2 - Cấu hình DHCP Server cho LAN và DMZ
- Thiết lập dịch vụ DHCP cấp phát IP tự động cho phân vùng LAN.
- Thiết lập dịch vụ DHCP cho phân vùng DMZ (hoặc IP tĩnh cho Server).
- Kiểm tra việc cấp phát IP thành công trên máy trạm Windows Server.
- Kết quả: PASS.

### TH3 - Cấu hình Firewall Rules (Tường lửa)
- Tạo các quy tắc cho phép lưu lượng từ LAN đi ra ngoài Internet.
- Tạo quy tắc kiểm soát/chặn phân vùng LAN truy cập trực tiếp vào vùng DMZ nhằm bảo mật.
- Hạn chế quyền truy cập từ vùng DMZ ngược về mạng nội bộ LAN.
- Kết quả: PASS.

### TH4 - Cấu hình NAT (Port Forwarding)
- Thiết lập quy tắc NAT Port Forwarding trên cổng WAN để chuyển tiếp dịch vụ (ví dụ: Web Server đặt ở vùng DMZ).
- Kiểm tra khả năng truy cập dịch vụ từ bên ngoài vào vùng DMZ an toàn.
- Kết quả: PASS.

### TH5 - Kiểm thử lưu lượng bằng Windows Server
- Đăng nhập máy ảo Windows Server thuộc mạng LAN[cite: 6].
- Sử dụng lệnh `ping`, `tracert` và trình duyệt web để kiểm thử thông mạch giữa các vùng mạng.
- Xác thực các quy tắc chặn/cho phép của Firewall hoạt động chính xác.
- Kết quả: PASS.

## Kết quả

| Tình huống | Kết quả |
|---|---|
| TH1: Cài đặt và Giao diện | PASS |
| TH2: DHCP Server | PASS |
| TH3: Firewall Rules | PASS |
| TH4: NAT Port Forwarding | PASS |
| TH5: Kiểm thử lưu lượng | PASS |

## Lỗi gặp phải và cách khắc phục

### Cấu hình Card mạng ảo (VMnet)
Lần đầu khởi động, pfSense không nhận diện đúng các cổng mạng do xung đột cấu hình card trong VMware.
Đã tiến hành gán lại thủ công các card mạng (em0, em1, em2) thông qua menu console số `1` của pfSense.

### Quy tắc Firewall Rules
Ban đầu để mặc định rule cho phép tất cả (Any to Any) dẫn đến chưa tối ưu tính bảo mật giữa LAN và DMZ.
Đã bổ sung thêm các rule `Block` cụ thể từ LAN sang DMZ và ngược lại trước rule `Allow` tổng quát.

## An toàn và phạm vi thực hành

- Chỉ thực hiện trên môi trường máy ảo hóa nội bộ (VMware Workstation/VirtualBox).
- Không ảnh hưởng đến hệ thống mạng thật bên ngoài.
- Không sử dụng thông tin nhạy cảm hay dữ liệu thực tế trong quá trình cấu hình lab.
