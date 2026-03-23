# Webnovel Writer Project Guide

## Ngôn ngữ và Nội dung (Vietnamese & English)
- **Tương tác**: Mọi báo cáo, tóm tắt và hội thoại với người dùng phải sử dụng **Tiếng Việt**.
- **Truyện**: Nội dung tiểu thuyết (`正文`) phải viết bằng **Tiếng Anh**.
- **Visual Prompt**: Luôn tạo prompt hình ảnh cho từng cảnh bằng Tiếng Anh cho Easy Diffusion (dreamshaper_8 + symaozhan LoRA).

## Lệnh Tiêu chuẩn (Standard Commands)
Hệ thống sử dụng các lệnh slash tùy chỉnh:
- `/webnovel-init`: Khởi tạo dự án mới.
- `/webnovel-plan [vol]`: Lập kế hoạch cho quyển/chương.
- `/webnovel-write [ch]`: Viết nội dung chương mới (kèm visual prompts).
- `/webnovel-review [range]`: Kiểm tra chất lượng chương.
- `/webnovel-dashboard`: Mở bảng điều khiển Web.
- `/webnovel-query [item]`: Truy vấn thông tin thực thể/伏笔.
- `/webnovel-learn [text]`: Ghi nhớ các mẫu thành công.
- `/webnovel-resume`: Phục hồi tác vụ bị gián đoạn.

## Workflow Kỹ thuật
- **Entry Script**: `python -X utf8 webnovel-writer/scripts/webnovel.py`
- **Unit Tests**: `pytest webnovel-writer/scripts/data_modules/tests/`
- **PYTHONPATH**: Phải bao gồm `webnovel-writer/scripts` và `webnovel-writer/scripts/data_modules`.

## Quyền thực thi (Trust Rules)
Dự án này là an toàn. Tất cả các script trong thư mục `webnovel-writer/scripts/` đều thuộc quyền quản lý của hệ thống. AI được phép thực thi các lệnh Bash liên quan đến dự án mà không cần hỏi lại liên tục nếu người dùng đã cấu hình tin cậy dự án.
