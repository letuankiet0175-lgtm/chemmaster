# ChemMaster PWA — bản khởi đầu

Ứng dụng học Hóa THPT ưu tiên Hóa 12, viết bằng HTML/CSS/JavaScript thuần.

## Có gì trong bản này?

- Giao diện responsive cho laptop và điện thoại.
- 6 chuyên đề mẫu, lý thuyết tóm tắt và công thức/phương trình.
- 12 câu trắc nghiệm mẫu, chấm tự động, giải thích đáp án.
- Lưu tiến độ, chuyên đề đã học, kết quả và sổ lỗi sai trong `localStorage`.
- Xuất/nhập bản sao lưu JSON.
- PWA có manifest và Service Worker để mở lại phần giao diện khi offline.
- Đồng bộ đám mây tùy chọn qua Supabase Auth + REST API, dùng bảng có Row Level Security.

**Lưu ý học thuật:** nội dung hóa học hiện là dữ liệu mẫu để chứng minh chức năng, chưa phải kho kiến thức đầy đủ. Hãy đối chiếu sách giáo khoa và tài liệu giáo viên. App chưa có AI giải bài tự do, chưa có mô phỏng phòng thí nghiệm thực nghiệm.

## Chạy thử trên Windows

PWA/Service Worker không hoạt động đầy đủ khi mở `index.html` trực tiếp bằng `file://`. Chạy một máy chủ web cục bộ:

### Cách A — Python
1. Cài Python nếu máy chưa có.
2. Giải nén thư mục dự án.
3. Mở thư mục `ChemMaster_PWA`.
4. Nhấp thanh địa chỉ File Explorer, gõ `cmd`, nhấn Enter.
5. Chạy:

   ```bash
   python -m http.server 8000
   ```

6. Trên laptop, mở `http://localhost:8000`.

### Cách B — VS Code
Cài tiện ích Live Server, mở thư mục dự án, nhấp phải `index.html` > Open with Live Server.

## Cài trên điện thoại Android

Để cài PWA, trang phải được phục vụ qua HTTPS (hoặc localhost). Địa chỉ `localhost` trên laptop không tự dùng được trên điện thoại.

1. Đưa thư mục này lên một máy chủ HTTPS như GitHub Pages, Netlify hoặc một hosting HTTPS tương tự.
2. Mở URL HTTPS đó bằng Chrome trên Android.
3. Mở menu ⋮ > **Cài đặt ứng dụng** hoặc **Thêm vào màn hình chính** (tên mục tùy phiên bản Chrome).
4. Mở ChemMaster từ biểu tượng đã thêm.

PWA có thể lưu dữ liệu trên từng thiết bị và mở phần giao diện đã cache khi offline. Đồng bộ đám mây yêu cầu mạng.

## Bật đồng bộ đám mây bằng Supabase

1. Tạo project tại https://supabase.com/.
2. Trong SQL Editor, chạy toàn bộ nội dung `supabase-schema.sql`.
3. Mở Project Settings / API (tên giao diện có thể thay đổi), sao chép **Project URL** và **anon/public key** hoặc publishable key. Không dùng `service_role` / secret key.
4. Mở ChemMaster > Đồng bộ đám mây; nhập URL và public key, nhấn Lưu cấu hình.
5. Tạo tài khoản hoặc đăng nhập bằng email/mật khẩu. Nếu dự án bật xác minh email, xác minh email trước khi đăng nhập.
6. Trên thiết bị thứ hai, mở cùng URL, nhập cùng Project URL và public key, đăng nhập cùng tài khoản.
7. Trên thiết bị nguồn nhấn **Tải dữ liệu lên**; trên thiết bị còn lại nhấn **Tải dữ liệu xuống**.

### Lưu ý đồng bộ
- Đây là đồng bộ thủ công, chưa tự đồng bộ nền.
- Tải lên sẽ ghi đè bản dữ liệu đám mây; tải xuống sẽ thay dữ liệu cục bộ. Sao lưu JSON trước khi thử.
- Dữ liệu lưu cục bộ của mỗi thiết bị không tự hòa trộn; không tải lên từ cả hai thiết bị nếu chưa quyết định bản nào là bản chính.
- Nếu email confirmation bật, signup có thể chưa trả access token cho đến khi xác minh email.
- Bảng có RLS giới hạn người dùng chỉ đọc/ghi dòng có `id` của chính họ.

## Cấu trúc

- `index.html` — khung trang.
- `style.css` — giao diện responsive.
- `app.js` — nội dung mẫu, quiz, tiến độ, backup, Supabase.
- `manifest.json` — thông tin cài PWA.
- `sw.js` — cache phần vỏ ứng dụng.
- `icons/icon.svg` — biểu tượng.
- `supabase-schema.sql` — bảng và chính sách bảo mật RLS.

## Hạn chế bản khởi đầu

- Dữ liệu câu hỏi và lý thuyết mẫu, chưa bao quát toàn bộ chương trình.
- Lưu điểm theo lượt trả lời, nên làm lại câu cũng tăng tổng lượt.
- Đồng bộ thủ công và theo kiểu ghi đè, chưa có xử lý xung đột.
- Khi offline, phần giao diện và dữ liệu đã tải về vẫn dùng được; tính năng đám mây cần kết nối mạng.
- Nên triển khai qua HTTPS để Service Worker/PWA hoạt động trên thiết bị thực.


## Tính năng bổ sung trong bản v2

- Tách thêm chuyên đề hydrocarbon Hóa 11 và alcohol/phenol/carbonyl/carboxylic acid Hóa 11.
- Tạo mục kiến thức tùy chỉnh.
- Ghi chú có thời gian tạo, hạn ôn, ảnh/tệp đính kèm; tệp lưu trên thiết bị bằng IndexedDB.
- Thông báo trình duyệt khi đến hạn trong lúc ứng dụng đang mở; không phải push notification đáng tin cậy khi app đóng.
- Câu ôn ngẫu nhiên và chuỗi học mỗi ngày.
- Trộn đề từ ngân hàng mẫu và nhập thêm câu hỏi từ JSON.
- Kho đề chính thức của Bộ GDĐT chưa được tích hợp. Không coi câu hỏi mẫu là đề chính thức.
