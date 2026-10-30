# Triển khai đăng nhập DMS

## 1. Cập nhật dịch vụ Apps Script
- Mở dự án Apps Script hiện đang nhận ảnh; thay toàn bộ Code.gs bằng Code.gs trong bộ này.
- Trong hàm provisionUser, sửa const username='admin' thành tên cần cấp (chữ thường không dấu, số, dấu chấm/gạch ngang/gạch dưới; tối đa 50 ký tự).
- Chọn hàm provisionUser ở thanh công cụ và bấm Run. Chỉ chủ dự án chạy thao tác này.
- Nhật ký thực thi hiển thị mật khẩu ngẫu nhiên. Lưu vào trình quản lý mật khẩu, gửi riêng cho người được cấp. Không đưa nhật ký/mật khẩu lên GitHub. Không thay mật khẩu ngẫu nhiên bằng mật khẩu ngắn tự chọn.
- Mỗi lần chạy lại cùng tên là đặt lại mật khẩu và thu hồi các phiên cũ. Để cấp người khác, đổi username rồi chạy lại.
- Triển khai > Quản lý bản triển khai > bút chì > Phiên bản mới > Triển khai. Giữ nguyên URL /exec, chạy bằng Tôi, truy cập Bất kỳ ai: dịch vụ tự kiểm tra tài khoản.
- Nếu từng tạo các bản triển khai khác cho dịch vụ nhận ảnh không có đăng nhập, lưu trữ/vô hiệu hóa chúng để tránh còn đường tải ảnh công khai.

## 2. Cập nhật GitHub
- Upload cả index.html và login.html vào thư mục gốc của repo tienpctq/dms. Giữ nguyên tên.
- Không upload Code.gs hoặc tài khoản/mật khẩu. File này chỉ dùng trong Apps Script.
- Commit changes, chờ Pages cập nhật rồi mở https://tienpctq.github.io/dms/.
- Đăng nhập với tài khoản vừa tạo. Phiên tồn tại tối đa 8 giờ, lưu trong sessionStorage của tab. Mỗi tài khoản chỉ giữ một phiên server; đăng nhập mới vô hiệu phiên trước.
- Xuất thử một ảnh, kiểm tra thư mục, sao chép link rồi đăng xuất. Kiểm tra lại bằng cửa sổ riêng tư.

## 3. Vô hiệu hóa một tài khoản
Trong Apps Script > Project Settings > Script Properties, tìm user:TEN_TAI_KHOAN và xóa thuộc tính đó. Các phiên còn lại của người đó sẽ bị từ chối. Để cấp lại/reset, chạy provisionUser như trên.

## Giới hạn bảo mật
- GitHub Pages và repo công khai vẫn cho tải mã HTML và ảnh mẫu; màn hình đăng nhập không bảo mật các tài nguyên tĩnh này. Muốn che cả website cần chuyển sang hosting có kiểm soát truy cập phía máy chủ.
- Dịch vụ Apps Script kiểm tra phiên trước khi ghi ảnh lên Drive, kể cả gọi trực tiếp không qua website.
- Ảnh xuất vẫn chia sẻ với bất kỳ ai có link theo yêu cầu trước đó. Đăng nhập không thu hồi link ảnh đã chia sẻ.
- Mật khẩu ngẫu nhiên có độ dài 64 ký tự; chỉ hash kèm salt được giữ ở Script Properties. Phiên có mã ngẫu nhiên, server lưu hash, hết hạn sau 8 giờ.
- Có hạn chế đăng nhập sai theo tên tài khoản và toàn dịch vụ; đây không phải hệ thống chống tấn công quy mô lớn. Chủ dự án cần quản lý quyền truy cập Apps Script và tài khoản Google.
- Đã kiểm thử logic bằng mô phỏng; chưa triển khai hoặc kiểm thử đăng nhập thực tế trên Google trong phiên làm việc này.
