# RoboCTB - Vietnam Robotics Challenge 2023 (Zero Carbon)

Mã nguồn của **RoboCTB** — đội thi đại diện cho trường **THPT Chuyên Thái Bình** tham gia giải đấu **VietNam Robotics Challenge 2023** với chủ đề *"Zero Carbon"*. 

Dự án này chứa toàn bộ mã nguồn điều khiển robot **JetBot**, sử dụng vi điều khiển ESP32 (mạch VIA Bánh Mì) kết hợp với tay cầm điều khiển PS2 không dây để tối ưu hóa hiệu suất thu thập và xử lý các khối carbon/năng lượng trên sân đấu.

---

## Tính năng nổi bật của Robot

### 1. Chi tiết Cơ khí & Cơ cấu
* **Khung gầm (Base xe):** Sử dụng các thanh nhôm định hình vuông khép kín, tối ưu trọng lượng bằng sàn Formex. Cơ chế di chuyển gồm 2 động cơ chính kết hợp 2 bi điều hướng giúp xe linh hoạt khi xoay tua tại chỗ.
* **Cơ cấu lấy bóng (Intake v3):** Sử dụng hệ thống trục carbon giảm tải kết hợp dây thít và cơ chế bung mở rộng khoang chứa bằng dây cao su đàn hồi khi motor kích hoạt. Tốc độ quay được nhân lên 4 lần nhờ bộ truyền động Pulley (20-80) và xích nhông.
* **Cơ cấu bắn bóng (Shooter v3):** Hệ thống shooter đối xứng sử dụng 2 trục carbon quay ngược chiều nhau thông qua bộ 4 bánh răng nhựa và pulley (20-60), giúp tối ưu ma sát và tăng tối đa động năng bắn bóng cao vào khu lưu trữ.

### 2. Điểm đặc sắc trong Lập trình
* **Áp dụng Clean Code:** Các biến và hàm điều khiển được đặt tên tường minh (`laybong()`, `banbong()`, `tuhuy()`), bố cục chia tách chức năng rõ ràng giúp dễ quản lý và debug.
* **Thuật toán ACC (Acceleration Control):** Tích hợp bộ kiểm soát gia tốc tự động bằng phần mềm (`#define ACC`). Động cơ không tăng/giảm tốc đột ngột mà thay đổi từ từ theo chu kỳ 100ms dựa trên ngưỡng biến thiên cho phép (`acc_pos` / `acc_neg`). Điều này giúp bảo vệ motor, chống quá tải nhiệt và giảm hiện tượng trượt bánh.
* **Ánh xạ chính xác (Mapping):** Chuyển đổi mượt mà tọa độ Joystick dạng số nguyên từ tay cầm PS2 sang dải xung PWM điều khiển động cơ từ `0` đến `4095`.

---

## Sơ đồ điều khiển Tay cầm PS2

Robot được cấu hình điều khiển trực quan qua Gamepad PS2 với sơ đồ nút bấm như sau:

| Nút bấm | Chức năng tương ứng | Chế độ hoạt động |
| :--- | :--- | :--- |
| **Joystick Trái** | Di chuyển Tiến / Lùi / Điều hướng Rẽ | Trục Y giảm độ nhạy 1.5x, Trục X giảm 4x |
| **Nút Tam Giác (TRIANGLE)** | Kích hoạt hệ thống cuốn lấy bóng vào khoang | Nhấn nhả (Press) |
| **Nút L2 (PSB_L2)** | Khởi động Động cơ bắn bóng + Servo lùa bóng | Nhấn giữ (Hold) |
| **Nút R2 (PSB_R2)** | Đảo chiều Động cơ bắn bóng + Servo đảo chiều đẩy bóng ra | Nhấn giữ (Hold) |
| **Nút Tròn (CIRCLE)** | Tạm dừng cơ chế bắn bóng khẩn cấp | Nhấn nhả (Press) |
| **Nút Vuông (SQUARE)** | Dừng riêng motor lấy bóng | Nhấn nhả (Press) |
| **Nút Lên (PAD_UP)** | Quay Servo 180° theo chiều âm để **Đóng cửa xả** | Nhấn nhả (Press) |
| **Nút Xuống (PAD_DOWN)** | Quay Servo 180° theo chiều dương để **Mở cửa xả** | Nhấn nhả (Press) |
| **Nút X (CROSS)** | **TỰ HỦY** - Dừng khẩn cấp toàn bộ hệ thống (Motor & Servo) | Nhấn nhả (Press) |

---

## Cấu hình phần cứng & Thư viện

### Ghép nối chân ESP32 (VIA Bánh Mì) với Đầu thu PS2:
* `PS2_DAT` (MISO) -> Pin `12`
* `PS2_CMD` (MOSI) -> Pin `13`
* `PS2_SEL` (SS)   -> Pin `15`
* `PS2_CLK` (SCK)  -> Pin `14`

### Thư viện bắt buộc:
Để biên dịch được code thành công, hãy cài đặt các thư viện sau qua Arduino Library Manager:
1.  **`Adafruit PWM Servo Driver Library`** (Điều khiển mạch mở rộng PCA9685 qua giao tiếp I2C).
2.  **`PS2X_lib`** (Hỗ trợ đọc dữ liệu từ tay cầm PS2 không dây).

---

## Hướng dẫn Cài đặt & Nạp code

1.  Tải hoặc sao chép kho lưu trữ này về máy tính của bạn:
```bash
    git clone [https://github.com/ivanhb21/RoboCTB-VRC2023.git](https://github.com/ivanhb21/RoboCTB-VRC2023.git)
    ```
2.  Mở tệp mã nguồn bằng **Arduino IDE**.
3.  Vào mục `Tools` -> `Board` -> Chọn đúng bo mạch **`ESP32 Dev Module`**.
4.  Kiểm tra xem các thư viện cần thiết đã được cài đặt đầy đủ chưa.
5.  Kết nối ESP32 với máy tính qua cáp USB, chọn đúng Cổng COM (Port) và tiến hành **Upload**.

---

## 👥 Về chúng tôi (RoboCTB & CTB STEAM CLUB)

Kho lưu trữ này được xây dựng, duy trì và quản lý trực tiếp bởi tôi -  **Chủ nhiệm CLB STEAM trường THPT Chuyên Thái Bình kiêm Đội trưởng (Captain) của đội tuyển RoboCTB**. 

Chúng tôi là tập hợp những cá nhân đam mê Khoa học Kỹ thuật, với sứ mệnh lan tỏa tinh thần giáo dục STEAM và chế tạo Robot đến với cộng đồng học sinh.

* *Slogan hành động:* **From Zero to Hero** 🌟
* *Thông điệp liên minh:* "Hãy cùng chung tay bảo vệ và gìn giữ màu xanh của thế giới, vì một ngày mai tươi sáng hơn." 🌿
