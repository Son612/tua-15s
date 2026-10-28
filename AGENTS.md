# AGENTS.md — Tua video 15s

Ghi chú của dự án, dùng chung cho **Claude** (qua `CLAUDE.md` → `@AGENTS.md`) và **ChatGPT/Codex**. Quy tắc chung nằm ở kho riêng `Son612/ai-rules`.

**Kho này CÔNG KHAI** (GitHub Pages: https://son612.github.io/cat-video/). Không ghi vào đây (và không đưa vào code) thông tin riêng của gia đình: tên, ngày sinh, địa chỉ Apps Script, mã PIN.

- Ghi chú mới về dự án: ghi vào file này, không ghi vào `CLAUDE.md`.
- **Đưa lên web = đẩy lên nhánh `main`** (GitHub Pages tự cập nhật sau ~1 phút). Trước khi đẩy: tăng số `CACHE` trong `sw.js` để iPhone/iPad tải bản mới.
- Từ 28/10/2026 mã nguồn nằm thẳng ở kho này (trước đó ở kho riêng `cloude-code` rồi chép sang bằng `dang-len-web.sh` — đã bỏ).

## Dự án
- `index.html` — app "Tua 15s" cho iPhone (PWA): chọn video, **tua nhanh cả video cho vừa đúng 15 giây** (`LEN`, 30 khung/giây `FPS`), không tiếng, cắt xong tự mở bảng Chia sẻ để lưu vào Ảnh. Một file HTML, không thư viện ngoài. (Bản đầu là cắt 15 giây — người dùng nhầm, đã đổi sang tua nhanh.)
  - Cách làm: tự đọc hộp MP4/MOV (`openMovie`), giải mã bằng WebCodecs `VideoDecoder` (HEVC/H.264, cần iOS 16.4+) chỉ những khung cần, chọn 450 khung đều theo thời gian, vẽ qua canvas rồi mã hoá lại H.264 (`VideoEncoder`), tự ghép file MP4 (`muxMp4`, chép `mvhd`/`tkhd` gốc để giữ ngày quay và hướng xoay). Khi nhảy qua đoạn bỏ phải `flush` + `reset` bộ giải mã (HEVC iPhone có khung mượn đoạn trước, không làm thì lỗi "Decoding error").
  - Lưu vào Ảnh: Safari không cho web tự ghi vào Ảnh. App thử tự mở bảng Chia sẻ; nếu iPhone chặn (tua lâu) thì hiện nút "Lưu vào Ảnh" to giữa màn hình (`saveBox`). Người dùng đã chọn giữ web + bấm 1 lần, không làm app native.
  - Thử trên máy: cấu hình `web-tinh` → `http://localhost:8765/`. Đăng lên web: tăng `CACHE` trong `sw.js` rồi đẩy lên `main`.
