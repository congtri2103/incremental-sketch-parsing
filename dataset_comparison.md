# Khảo sát dataset cho bài toán Incremental Stroke Parsing

## 1. So sánh 3 nguồn dữ liệu

| Tiêu chí | Google Quick, Draw! | CROHME | TU-Berlin Sketch |
| :--- | :--- | :--- | :--- |
| **Link** | https://github.com/googlecreativelab/quickdraw-dataset | https://www.isical.ac.in/~crohme/ | https://huggingface.co/datasets/sdiaeyu6n/tu-berlin |
| **Định dạng** | `.ndjson`, `.npy`, `.bin` | `.inkml` | `.png` (bản trên Hugging Face) |
| **Tọa độ điểm** | Có `(x, y)` | Có `(x, y)` | Không |
| **Thứ tự nét** | Có  | Có  | Không |
| **Timestamp** | Có  | Có (tùy tập) | Không |
| **Nhãn** | Có 345 loại hình vẽ | Có từng ký hiệu toán | Có 250 loại vật thể |
| **Quan hệ không gian** | Không | Có cấu trúc MathML | Không |
| **Giấy phép** | CC BY 4.0 | Dùng cho nghiên cứu học thuật | CC BY 4.0 |

## 2. Ảnh Raster và dữ liệu Stroke có thứ tự
- **Ảnh Raster (`.png`, `.jpg`):** Chỉ là ảnh tĩnh, ví dụ bản TU-Berlin trên Hugging Face. Khi nhìn vào ảnh đã vẽ xong thì không thể biết chắc nét nào vẽ trước, nét nào vẽ sau, nhất là chỗ các nét cắt nhau. Vì vậy không dùng ảnh tĩnh cho bài toán phân tích nét theo thời gian được.
- **Dữ liệu Stroke (`.ndjson`, `.inkml`):** Lưu lại tọa độ từng điểm theo đúng thứ tự lúc người vẽ đặt bút và có mốc thời gian (`timestamp`). Dữ liệu này mới dùng để chạy bài toán nhận diện từng bước được.

## 3. Giấy phép và quyền sử dụng
- **Quick, Draw!:** Giấy phép CC BY 4.0, được tải về, xử lý và chia sẻ lại thoải mái miễn là ghi nguồn. Dữ liệu tải trực tiếp từ kho của Google nên không cần vào web .
- **CROHME:** Dùng cho mục đích nghiên cứu/học thuật, cần trích dẫn nguồn khi dùng và không tự ý up lại toàn bộ bộ dữ liệu gốc ra ngoài.
- **TU-Berlin:** Link gốc của trường (`cybertron.cg.tu-berlin.de`) bị lỗi không vào được nên xem qua bản lưu trên Hugging Face (giấy phép CC BY 4.0).

## 4. Chọn dataset chính và hạn chế
- **Dataset chính:** Chọn **Google Quick, Draw! (bản raw `.ndjson`)**.
- **Lý do:** Có đầy đủ dữ liệu thật về thứ tự điểm, thứ tự nét và `timestamp`. File `.ndjson` dễ dùng Python (`json`) đọc trực tiếp để làm tiếp phần tiền xử lý ở Ngày 2, đỡ mất công viết bộ đọc thẻ XML phức tạp như file `.inkml` của CROHME. Giấy phép CC BY 4.0 cũng tiện để nhóm chia sẻ tập mẫu trên repo.
- **Trường bị thiếu / Hạn chế:** Bộ này không có nhãn quan hệ không gian giữa các chi tiết nhỏ bên trong hình (ví dụ bức vẽ cái nhà chỉ có 1 nhãn chung là `house`, không tách riêng đâu là mái hay cửa). Nếu sau này cần phân tích cấu trúc chi tiết thì sẽ lấy thêm mẫu từ CROHME.

## 5. Cách lấy tập thử 100–200 mẫu (tái lập được)
- Chọn cố định 4 chủ đề (`cat`, `house`, `fish`, `apple`).
- Dùng Python đọc file `.ndjson`, đặt cố định `random.seed(42)` rồi lấy ngẫu nhiên 40 mẫu mỗi loại (tổng 160 mẫu).
- Lưu lại danh sách `key_id` của 160 mẫu này ra một file riêng để sau này ai chạy lại code cũng lấy đúng từng đó mẫu.
