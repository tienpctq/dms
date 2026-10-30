# Kết nối Google Drive cho DMS

Nút “Xuất ảnh lên Drive” đã có mã tải JPG, chọn thư mục, chia sẻ chỉ xem và sao chép liên kết. Chưa thể dùng cho đến khi hoàn tất cấu hình dưới đây. Chưa kiểm tra tải thật với tài khoản của bạn.

## Website GitHub Pages

Mở https://tienpctq.github.io/dms/ . Trong Google OAuth, thêm Authorized JavaScript origin: https://tienpctq.github.io ; giới hạn website của Google Picker API key: https://tienpctq.github.io/* . Không cần chạy localhost khi dùng website.

## 1. Mở tool qua localhost

Giữ ba tệp Chinh-bang-thiet-bi.html, Chay-DMS-local.cjs và Mo-DMS.cmd trong cùng thư mục. Máy này đã có Node.js. Nhấp đúp Mo-DMS.cmd, giữ cửa sổ đó mở, rồi mở http://localhost:8765 bằng Chrome hoặc Edge. Đóng cửa sổ để dừng máy chủ. Máy chủ chỉ nghe trên máy này, không công khai ra mạng.

Dữ liệu trình duyệt ở địa chỉ file và localhost độc lập: trước khi chuyển, dùng “Lưu dữ liệu” ở bản cũ rồi “Mở dữ liệu” ở localhost.

## 2. Cấu hình Google một lần

1. Mở https://console.cloud.google.com/ và tạo/chọn dự án của bạn.
2. Trong APIs & Services, bật Google Drive API và Google Picker API.
3. Trong Google Auth Platform, cấu hình tên ứng dụng, email liên hệ, Audience External. Khi dùng chế độ Testing, thêm email Google cá nhân của bạn vào Test users.
4. Tạo OAuth Client ID loại Web application. Thêm Authorized JavaScript origin: http://localhost:8765 (không thêm đường dẫn). Sao chép Client ID. Không cần nhập Client Secret vào tool.
5. Tạo API key trong cùng dự án. Giới hạn API key cho Google Picker API và website http://localhost:8765/*.
6. Trong tool, mở “Thư mục lưu ảnh trên Google Drive”, nhập Client ID và API key, bấm “Lưu cấu hình Google”.

## 3. Tải và chia sẻ

- Bấm “Xuất ảnh lên Drive”, đăng nhập tài khoản Google và cấp quyền drive.file. Đây là quyền với tệp ứng dụng tạo và các tệp/thư mục bạn chọn, không phải toàn bộ Drive.
- Lần đầu, chọn thư mục đích trong cửa sổ Google Picker để cấp quyền cho thư mục. Tool đã điền sẵn đường dẫn bạn cung cấp, nhưng chỉ có đường dẫn thì chưa có quyền truy cập API.
- Tool tải JPG rồi đặt quyền “Bất kỳ ai có liên kết đều có thể xem”. Bấm “Sao chép link”.
- Nếu ảnh đã tải nhưng chia sẻ thất bại, tool giữ lại link và báo rõ; mở ảnh trên Drive để sửa quyền, không cần tải lại.
- Mỗi lần xuất tạo một ảnh mới, không ghi đè ảnh cũ. Chỉ Client ID, API key và đường dẫn thư mục được lưu trên trình duyệt; access token chỉ giữ trong bộ nhớ của phiên.

Tài liệu Google:
- https://developers.google.com/identity/oauth2/web/guides/use-token-model
- https://developers.google.com/workspace/drive/api/guides/manage-uploads
- https://developers.google.com/workspace/drive/picker/guides/overview

