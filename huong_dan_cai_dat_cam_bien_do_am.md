# Hướng dẫn Cài đặt Cảm biến Độ ẩm cho Trang trại Lan

## Phần 1: Báo cáo phân tích Input / Output (I/O)

Tài liệu này định nghĩa rõ cách thức chuyển đổi từ văn bản thô (Input) sang giao diện render chuyên nghiệp (Output) khi sử dụng định dạng Markdown cho hệ thống trang trại Lan.

*   **Input (Văn bản thô):** Các ký tự mã nguồn Markdown sử dụng dấu backtick (\`) cho inline code, khối mã ba dấu backtick (\`\`\`) kèm tên ngôn ngữ cho code block, ký tự gạch đứng (`|`) và gạch ngang (`-`) để dựng bảng, cùng các ký tự phân tách task list (`[ ]` và `[x]`). Ngoài ra, sử dụng ký tự escape `\` để giữ nguyên các ký tự đặc biệt như dấu sao `\*` hoặc dấu thăng `\#` khi không muốn chúng bị dịch thành định dạng.
*   **Output (Giao diện render chuyên nghiệp):** Trình duyệt hoặc công cụ đọc Markdown sẽ hiển thị:
    *   Tên biến, lệnh Terminal dưới dạng phông chữ đơn không gian (monospace) có nền xám nổi bật.
    *   Đoạn mã lập trình Python có tính năng tô sáng cú pháp (Syntax Highlighting) giúp dễ dàng đọc các từ khóa, hàm và chuỗi.
    *   Bảng dữ liệu cấu trúc rõ ràng, căn chỉnh đẹp mắt để so sánh các thông số kỹ thuật.
    *   Danh sách công việc tương tác (Task List) với các hộp kiểm (checkbox) có thể đánh dấu trực quan.

---

## Phần 2: Tài liệu Hướng dẫn Kỹ thuật

Để bắt đầu cấu hình hệ thống, trước tiên bạn cần cập nhật các gói phần mềm trên bo mạch điều khiển bằng cách mở Terminal và gõ lệnh `sudo apt update`. Sau đó, kiểm tra biến môi trường `SENSOR_PIN` để đảm bảo kết nối chân GPIO chính xác. Tránh sử dụng nhầm các ký tự điều khiển định dạng như hiển thị chuỗi chứa dấu đặc biệt, ví dụ: định dạng ký tự \`#pin_config\` hoặc \`\*sensor_active\*\`.

### 1. Mã nguồn điều khiển máy bơm (Python)

Dưới đây là đoạn mã Python sử dụng thư viện để đọc giá trị độ ẩm và kích hoạt máy bơm khi độ ẩm xuống thấp hơn ngưỡng cho phép:

```python
import time

# Khai báo biến cấu hình hệ thống
SENSOR_PIN = 4
THRESHOLD_MOISTURE = 30.5

def check_moisture_level(current_value):
    """Kiểm tra độ ẩm và điều khiển máy bơm."""
    if current_value < THRESHOLD_MOISTURE:
        print(f"Độ ẩm thấp ({current_value}%). Kích hoạt máy bơm tại chân GPIO {SENSOR_PIN}...")
        return True
    print("Độ ẩm ổn định. Hệ thống ở chế độ chờ.")
    return False

if __name__ == "__main__":
    # Giả lập vòng lặp đọc dữ liệu cảm biến
    sample_moisture = 25.0
    check_moisture_level(sample_moisture)
```

### 2. Bảng so sánh loại cảm biến độ ẩm

| Tiêu chí so sánh | Cảm biến cũ (Model A) | Cảm biến mới (Model B) |
| :--- | :--- | :--- |
| **Độ chính xác** | Trung bình ($\pm 5\%$) | Cao ($\pm 1\%$) |
| **Tuổi thọ trung bình** | 6 tháng - 1 năm | 2 - 3 năm |
| **Giao tiếp dữ liệu** | Analog (`A0`) | Digital (`I2C`) |
| **Khả năng chống ẩm** | Thấp | Chuẩn công nghiệp |

### 3. Danh sách kiểm tra thiết bị (Task List)

- [x] Kiểm tra nguồn điện và dây cáp kết nối bo mạch chính
- [x] Cài đặt các thư viện phụ thuộc bằng lệnh `pip install -r requirements.txt`
- [ ] Hiệu chuẩn lại giá trị đọc của cảm biến độ ẩm đất
- [ ] Chạy thử nghiệm chương trình tự động tưới nước trong 15 phút
- [ ] Báo cáo kết quả vận hành cho quản lý trang trại