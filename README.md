# Hệ thống Phân loại Màu sắc bằng Edge AI (WebAssembly)

Kho lưu trữ này chứa toàn bộ mã nguồn của hệ thống Trí tuệ nhân tạo tại biên (Edge AI) phục vụ cho bài toán phân loại khối lập phương theo 4 màu sắc cơ bản: `red` (đỏ), `green` (xanh lá), `blue` (xanh dương), và `yellow` (vàng). 

Mô hình học máy (MobileNetV2) được huấn luyện trên nền tảng Edge Impulse, lượng tử hóa Int8 và xuất dưới định dạng WebAssembly để thực thi suy luận thời gian thực (Real-time Inference) trực tiếp trên trình duyệt.

## 📂 Cấu trúc thư mục

Dự án bao gồm các tệp tin cốt lõi sau để khởi chạy mô hình trên môi trường Web:

- `index.html`: Giao diện người dùng chính. Chứa logic xin quyền truy cập Camera của thiết bị, hiển thị luồng video (video stream) và in kết quả dự đoán (Confidence score) ra màn hình.
- `edge-impulse-standalone.wasm`: Tệp nhị phân WebAssembly chứa trọng số (weights) của mạng nơ-ron MobileNetV2 đã được tối ưu hóa.
- `edge-impulse-standalone.js`: Cầu nối (wrapper) JavaScript giúp trình duyệt giao tiếp và gọi các hàm tính toán từ tệp `.wasm`.
- `run-impulse.js`: Tập lệnh tiền xử lý hình ảnh (crop, resize về 96x96 pixel) và đẩy dữ liệu vào mô hình để suy luận.
- `server.py`: Mã nguồn Python hỗ trợ tạo Local Server nhanh (dùng để vượt qua chính sách bảo mật CORS của trình duyệt khi tải tệp `.wasm`).
- `README.md`: Tệp tài liệu hướng dẫn sử dụng (tệp này).

## 🚀 Hướng dẫn cài đặt và chạy thử nghiệm

Do chính sách bảo mật của các trình duyệt hiện đại (CORS policy), không thể mở trực tiếp tệp `index.html` bằng cách nhấp đúp chuột. Vui lòng chạy dự án thông qua một Local Server theo một trong hai cách dưới đây:

### Cách 1: Sử dụng Python (Khuyên dùng)
Yêu cầu: Máy tính đã cài đặt sẵn Python 3.
1. Tải toàn bộ mã nguồn này về máy (Download ZIP hoặc dùng lệnh `git clone`).
2. Mở Terminal (hoặc Command Prompt) tại thư mục vừa giải nén.
3. Khởi chạy máy chủ cục bộ bằng lệnh sau:
```bash
   python server.py
   # hoặc dùng lệnh: python3 server.py (đối với macOS/Linux)
```
4. Mở trình duyệt web (Chrome, Edge, Safari) và truy cập vào địa chỉ: `http://localhost:8000`
5. Cấp quyền truy cập Camera khi trình duyệt yêu cầu để bắt đầu kiểm thử nhận diện màu sắc theo thời gian thực.

### Cách 2: Sử dụng VS Code (Live Server)
1. Mở thư mục chứa mã nguồn bằng phần mềm Visual Studio Code.
2. Cài đặt tiện ích mở rộng (Extension) mang tên **Live Server** của tác giả Ritwick Dey.
3. Nhấp chuột phải vào tệp `index.html` và chọn **"Open with Live Server"**.
4. Trình duyệt sẽ tự động bật lên. Hãy cấp quyền Camera và sử dụng hệ thống.

---
**Lưu ý kiểm thử:** Để mô hình đạt độ chính xác cao nhất (có thể lên tới 1.00), vui lòng đặt vật thể trên phông nền ít chi tiết và đảm bảo điều kiện ánh sáng phòng phân bổ đều, không bị lóa sáng mạnh.
