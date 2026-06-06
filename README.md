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

### 🛠️ Phương thức 1: Kiểm thử Camera trực tiếp qua Edge Impulse (Khuyên dùng)
Do chính sách bảo mật API nghiêm ngặt của nền tảng đối với các dự án công khai, giảng viên vui lòng thực hiện tuần tự theo 5 bước sau để kích hoạt luồng camera trên thiết bị di động cá nhân:

1. **Truy cập dự án công khai:** Truy cập vào liên kết không gian làm việc của dự án tại địa chỉ: [https://studio.edgeimpulse.com/studio/1018230]
2. **Sao chép dự án (Clone Project):** Bấm vào nút **"Clone project"** ở góc trên bên phải màn hình để đồng bộ toàn bộ tập dữ liệu (dataset) và cấu trúc mạng nơ-ron về tài khoản cá nhân.
3. **Thiết lập kết nối Client:** Tại giao diện dự án vừa sao chép, di chuyển đến mục **Devices** (ở thanh menu bên trái) -> Chọn **Connect a new device** -> Chọn tiếp **Use your mobile phone**.
4. **Quét mã kiểm thử:** Sử dụng camera của điện thoại quét mã QR hiển thị trên màn hình máy tính để trình duyệt cấp quyền truy cập phần cứng.
5. **Thực thi suy luận thời gian thực:** Trên giao diện trình duyệt điện thoại vừa mở ra, cuộn xuống dưới cùng và bấm chọn nút **"Switch to classification mode"** (Chuyển sang chế độ phân loại). Tiến hành hướng ống kính vào các khối màu lập phương để kiểm chứng độ chính xác (Confidence score đạt từ 0.95 đến 1.00) và tốc độ xử lý biên tối ưu (~1ms).

### Phương thức 2: Kiểm chứng mã nguồn WebAssembly chạy Offline (Developer Mode)
Đây là mã nguồn chứng minh mô hình nơ-ron đã được lượng tử hóa và có thể chạy hoàn toàn độc lập không cần Internet.
1. Tải toàn bộ mã nguồn về máy và khởi chạy Local Server:
   `python server.py` (hoặc `python3 server.py`)
2. Mở trình duyệt web truy cập: `http://localhost:8000`
3. Để kiểm tra bộ não AI tính toán offline, vui lòng lấy một chuỗi "Raw features" (đặc trưng ma trận ảnh) từ nền tảng Edge Impulse, dán vào ô trống và bấm "Run inference". Hệ thống sẽ trả về kết quả JSON với tốc độ ~1ms.
