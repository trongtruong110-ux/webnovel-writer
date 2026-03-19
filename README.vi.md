# Webnovel Writer

[![License](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://www.python.org/)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Compatible-purple.svg)](https://claude.ai/claude-code)

<a href="https://trendshift.io/repositories/22487" target="_blank"><img src="https://trendshift.io/api/badge/repositories/22487" alt="lingfengQAQ%2Fwebnovel-writer | Trendshift" style="width: 250px; height: 55px;" width="250" height="55"/></a>

[简体中文](./README.md) | [English](./README.en.md)

## Giới thiệu

`Webnovel Writer` là một hệ thống sáng tác tiểu thuyết mạng dài kỳ dựa trên Claude Code. Mục tiêu là giảm bớt tình trạng "quên cốt truyện" và "ảo giác" của AI trong quá trình viết, đồng thời hỗ trợ sáng tác dài hạn.

Tài liệu chi tiết đã được chia nhỏ trong thư mục `docs/`:

- Kiến trúc và các Mô-đun: `docs/architecture.md`
- Chi tiết các Lệnh: `docs/commands.md`
- RAG và Cấu hình: `docs/rag-and-config.md`
- Mẫu Thể loại: `docs/genres.md`
- Vận hành và Phục hồi: `docs/operations.md`
- Điều hướng Tài liệu: `docs/README.md`

## Yêu cầu Hệ thống Tối thiểu

Vì các logic AI cốt lõi (Claude) và RAG (Embedding/Rerank) đều dựa trên việc gọi API, yêu cầu phần cứng máy cục bộ là rất thấp:

- **CPU**: 2+ Nhân (Khuyên dùng để xử lý RAG song song)
- **Bộ nhớ (RAM)**: 4GB+ (Để chạy Claude Code và Dashboard cùng lúc)
- **GPU**: Không yêu cầu GPU cục bộ (Tất cả việc suy luận đều được thực hiện qua API)
- **Lưu trữ**: 1GB+ dung lượng trống (Cho dữ liệu dự án, cơ sở dữ liệu vector và các phụ thuộc)
- **Mạng**: Kết nối internet ổn định để truy cập API

## Bắt đầu nhanh

### 1) Cài đặt Plugin (Marketplace chính thức)

```bash
claude plugin marketplace add lingfengQAQ/webnovel-writer --scope user
claude plugin install webnovel-writer@webnovel-writer-marketplace --scope user
```

> Nếu chỉ muốn áp dụng cho dự án hiện tại, hãy đổi `--scope user` thành `--scope project`.

### 2) Cài đặt các phụ thuộc Python

```bash
python -m pip install -r https://raw.githubusercontent.com/lingfengQAQ/webnovel-writer/HEAD/requirements.txt
```

Lưu ý: Lệnh này sẽ cài đặt cả chuỗi viết cốt lõi và các phụ thuộc cho Dashboard.

### 3) Khởi tạo dự án tiểu thuyết

Thực hiện trong Claude Code:

```bash
/webnovel-init
```

Lưu ý: `/webnovel-init` sẽ tạo một `PROJECT_ROOT` (thư mục con) trong Workspace hiện tại và ghi con trỏ dự án hiện tại vào `workspace/.claude/.webnovel-current-project`.

### 4) Cấu hình môi trường RAG (Bắt buộc)

Vào thư mục gốc của dự án tiểu thuyết đã khởi tạo, tạo tệp `.env`:

```bash
cp .env.example .env
```

Ví dụ cấu hình tối thiểu:

```bash
EMBED_BASE_URL=https://api-inference.modelscope.cn/v1
EMBED_MODEL=Qwen/Qwen3-Embedding-8B
EMBED_API_KEY=your_embed_api_key

RERANK_BASE_URL=https://api.jina.ai/v1
RERANK_MODEL=jina-reranker-v3
RERANK_API_KEY=your_rerank_api_key
```

### 5) Bắt đầu sử dụng

```bash
/webnovel-plan 1
/webnovel-write 1
/webnovel-review 1-5
```

Nếu cần khắc phục các vấn đề về CLI cục bộ / thư mục plugin / phân giải thư mục gốc dự án, bạn có thể chạy kiểm tra tiền kiểm thống nhất:

```bash
python -X utf8 "<CLAUDE_PLUGIN_ROOT>/scripts/webnovel.py" --project-root "<WORKSPACE_ROOT>" preflight
```

### 6) Khởi động bảng điều khiển trực quan (Tùy chọn)

```bash
/webnovel-dashboard
```

Lưu ý:
- Dashboard là bảng điều khiển chỉ đọc (Trạng thái dự án, sơ đồ thực thể, duyệt chương/đề cương, theo dõi lực hút độc giả).
- Các sản phẩm build frontend đã được phát hành cùng với plugin; người dùng không cần chạy `npm build` cục bộ.

### 7) Cài đặt mô hình Agent (Tùy chọn)

Tất cả các Agent tích hợp sẵn trong dự án mặc định được cấu hình là:

```yaml
model: inherit
```

Nghĩa là Agent con sẽ kế thừa mô hình đang được sử dụng trong phiên Claude hiện tại.

Nếu muốn chỉ định mô hình riêng cho một Agent nào đó, hãy chỉnh sửa frontmatter của tệp tương ứng (`webnovel-writer/agents/*.md`), ví dụ:

```yaml
---
name: context-agent
description: ...
tools: Read, Grep, Bash
model: sonnet
---
```

Các giá trị phổ biến: `inherit` / `sonnet` / `opus` / `haiku` (tùy thuộc vào sự hỗ trợ của Claude Code hiện tại).

## Lịch sử Phiên bản

| Phiên bản | Mô tả |
|------|------|
| **v5.5.4 (Hiện tại)** | Bổ sung các ràng buộc mạnh mẽ cho lời nhắc chuỗi viết; thống nhất nội dung đánh giá/trau chuốt/báo cáo Agent hướng tới tiếng Trung; dọn dẹp số phiên bản nội bộ. |
| **v5.5.3** | Thêm lệnh `preflight` thống nhất; thống nhất các ví dụ CLI chuỗi viết sang phương thức chạy UTF-8 để giảm rủi ro lỗi mã hóa trên Windows. |
| **v5.5.2** | Hỗ trợ đồng bộ hóa tên chương từ đề cương chi tiết sang tên tệp văn bản; sửa lỗi tương thích workflow_manager. |
| **v5.5.1** | Sửa lỗi trích xuất chương trong đề cương tệp đơn cấp quyển; bổ sung tài liệu cho `/webnovel-dashboard` và `/webnovel-learn`. |
| **v5.5.0** | Thêm Skill Dashboard trực quan chỉ đọc (`/webnovel-dashboard`) với khả năng làm mới thời gian thực; hỗ trợ khởi động từ thư mục plugin. |
| **v5.4.4** | Giới thiệu cơ chế cài đặt Plugin Marketplace chính thức; thống nhất các lệnh gọi CLI cho Skills/Agents/References. |
| **v5.4.3** | Tăng cường hỗ trợ ngữ cảnh RAG thông minh. |
| **v5.3** | Giới thiệu hệ thống "Lực hút độc giả" (Hook / Cool-point / Theo dõi nợ). |

## Phát hành Plugin

Khuyên dùng quy trình `Plugin Release` của GitHub Actions để phát hành thống nhất:

1. Đồng bộ thông tin phiên bản tại cục bộ:
   ```bash
   python -X utf8 webnovel-writer/scripts/sync_plugin_version.py --version 5.5.4 --release-notes "Ghi chú phiên bản này"
   ```
2. Commit và push các thay đổi phiên bản (`README.md`, `plugin.json`, `marketplace.json`).
3. Mở trang Actions của kho lưu trữ, chọn `Plugin Release`.
4. Nhập `version` (ví dụ `5.5.4`) và `release_notes`.
5. Quy trình làm việc sẽ:
   - Kiểm tra tính nhất quán của phiên bản.
   - Tạo và push Tag `vX.Y.Z`.
   - Tạo GitHub Release.

## Giấy phép
Dự án này sử dụng giấy phép `GPL v3`, xem tệp `LICENSE` để biết chi tiết.

## Lịch sử Star

[![Star History Chart](https://api.star-history.com/svg?repos=lingfengQAQ/webnovel-writer&type=Date)](https://star-history.com/#lingfengQAQ/webnovel-writer&Date)

## Lời cảm ơn

Dự án này được phát triển bằng cách sử dụng **Claude Code + Gemini CLI + Codex** kết hợp với phương thức Vibe Coding.
Nguồn cảm hứng: [Bài đăng trên Linux.do](https://linux.do/t/topic/1397944/49)

## Đóng góp

Chào mừng các Issue và PR:

```bash
git checkout -b feature/your-feature
git commit -m "feat: add your feature"
git push origin feature/your-feature
```
