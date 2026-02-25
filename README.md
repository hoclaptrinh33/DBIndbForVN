# DBIndbForVN

Dự án cơ sở dữ liệu Oracle cho hệ thống quản lý phim (tương tự IMDb) dành cho thị trường Việt Nam.

## Cấu trúc cơ sở dữ liệu

Dự án bao gồm các bảng chính sau:

1. **Lookup Tables**: `countries`, `languages`
2. **Titles**: `titles` (Movie, Series, Episode)
3. **Series/Season/Episode**: `series`, `seasons`, `episodes`
4. **Studios**: `studios`, `title_studios`
5. **Persons**: `persons`
6. **Cast & Crew**: `roles`, `title_person_roles`
7. **Users**: `users`
8. **Reviews**: `reviews`
9. **Audit Log**: `title_audit_log`

## Tính năng nổi bật

- **Chuẩn hóa 3NF**: Đảm bảo tính toàn vẹn dữ liệu.
- **Denormalization có chủ đích**: Tối ưu hóa hiệu suất truy vấn cho điểm đánh giá trung bình (`avg_rating`).
- **Triggers**: Tự động cập nhật điểm đánh giá và ghi log thay đổi dữ liệu.
- **Packages**: Đóng gói các nghiệp vụ chính như thêm phim, thêm đánh giá.
- **Phân quyền**: Quản lý quyền truy cập với các role `role_content_manager` và `role_viewer`.

## Cách sử dụng

1. Chạy script SQL trong thư mục `oracle-db-creator/references/-- 1.sql` để tạo cấu trúc cơ sở dữ liệu, trigger, package và dữ liệu mẫu.
2. Sử dụng các user đã được tạo sẵn (`viewer_01`, `content_mgr_01`) để kiểm tra phân quyền.
3. Chạy các đoạn script test ở cuối file để kiểm tra các nghiệp vụ.