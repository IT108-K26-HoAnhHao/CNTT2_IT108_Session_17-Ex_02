# BÀI 2

## PHẦN 1: PHÂN TÍCH SỰ CỐ

### 1. Tại sao An sửa file CSS thành màu đỏ nhưng khi F5 trình duyệt vẫn hiện màu xanh cũ?
* **Nguyên nhân do Browser Cache (Bộ nhớ đệm):** Trình duyệt lưu trữ các tệp tĩnh (như hình ảnh, file CSS, file JavaScript) trong bộ nhớ đệm để tăng tốc độ tải trang ở những lần truy cập sau.
* Khi An nhấn phím **`F5`** (Normal Reload), trình duyệt đôi khi vẫn ưu tiên lấy file CSS cũ đang được lưu trữ cục bộ trong bộ nhớ đệm thay vì tải tệp mới nhất từ Server, dẫn đến việc nút *"Thanh toán"* vẫn hiển thị màu xanh cũ.

### 2. Tại sao An đóng trình duyệt và mở lại lại bị văng ra khỏi hệ thống (Session Conflict / Mất phiên đăng nhập)?
* **Nguyên nhân do sử dụng Session Cookies:** Hệ thống đang cấp phát các mẩu tin định danh dưới dạng **Session Cookies** mà không cấu hình thời gian tồn tại (*Max-Age* hoặc *Expires*).
* Bản chất của **Session Cookies** là chỉ tồn tại trong suốt phiên làm việc của trình duyệt. Ngay khi người dùng đóng cửa sổ hoặc tắt hẳn trình duyệt, toàn bộ các cookie này sẽ bị xóa sạch tự động để đảm bảo bảo mật. Do đó, khi An mở lại trình duyệt, hệ thống không còn nhận diện được mã định danh và buộc An phải đăng nhập lại từ đầu.

---

## PHẦN 2: ĐỀ XUẤT GIẢI PHÁP SỬA LỖI

### 1. Khắc phục sự cố dính giao diện CSS cũ (Lỗi Cache)
* **Hành động 1 (Thao tác trên trình duyệt):** Sử dụng tổ hợp phím cưỡng chế tải lại **`Ctrl + Shift + R`** (Hard Reload) hoặc mở tab *Network* trong DevTools (F12) rồi tích chọn ô **Disable Cache** để trình duyệt bắt buộc tải toàn bộ mã nguồn mới nhất từ Server.
* **Hành động 2 (Giải pháp lập trình):** Thêm cơ chế phiên bản hoặc query string vào đường dẫn nhúng file CSS trên mã nguồn (ví dụ: `style.css?v=1.0.1`) để buộc trình duyệt hiểu đây là tệp mới và tự động tải lại mỗi khi có thay đổi.

### 2. Khắc phục sự cố mất phiên đăng nhập (Lỗi Cookie)
* **Hành động cụ thể:** Chuyển đổi cơ chế lưu trữ mẩu tin định danh từ **Session Cookies** sang **Persistent Cookies** (Cookies dài hạn) bằng cách thiết lập rõ ràng thuộc tính **`Max-Age`** hoặc **`Expires`** ở phía Backend khi tạo Cookie. 
* *Ví dụ cấu hình:* Gán thời gian sống cho Cookie kéo dài 7 ngày hoặc 30 ngày (`Max-Age: 604800`), giúp thông tin đăng nhập của An vẫn được lưu trữ an toàn ngay cả khi đã tắt trình duyệt, tránh việc bị văng phiên liên tục.

