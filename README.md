# Hệ thống Phân loại Màu sắc bằng Edge AI (WebAssembly)
## 👤 Sinh viên thực hiện
*   **Họ và tên: Nguyễn Bình An
*   **MSSV: N23DCCI001

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

## 🚀 Hướng dẫn kiểm thử hệ thống

Để thuận tiện cho việc đánh giá, quá trình kiểm thử được chia làm 2 phương thức: Kiểm thử thực tế qua Camera và Kiểm chứng mã nguồn nén tại biên.

### Phương thức 1: Kiểm thử Camera trực tiếp (Khuyên dùng)
Hệ thống đã được cấu hình sẵn để test thời gian thực thông qua Mobile Client của Edge Impulse.
1. Truy cập vào dự án Edge Impulse công khai của sinh viên tại đường link: **https://studio.edgeimpulse.com/studio/1018230**
2. Chuyển sang tab **Live classification**.
3. Sử dụng điện thoại quét mã QR hoặc kết nối thiết bị để hệ thống mở luồng Camera và tiến hành nhận diện 4 khối màu thực tế.

### Phương thức 2: Kiểm chứng mã nguồn WebAssembly chạy Offline (Developer Mode)
Đây là mã nguồn chứng minh mô hình nơ-ron đã được lượng tử hóa và có thể chạy hoàn toàn độc lập không cần Internet.
1. Tải toàn bộ mã nguồn về máy và khởi chạy Local Server:
   `python server.py` (hoặc `python3 server.py`)
2. Mở trình duyệt web truy cập: `http://localhost:8000`
3. Để kiểm tra bộ não AI tính toán offline, vui lòng lấy một chuỗi "Raw features" (đặc trưng ma trận ảnh) từ nền tảng Edge Impulse, dán vào ô trống và bấm "Run inference". Hệ thống sẽ trả về kết quả JSON với tốc độ ~1ms.
