# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202604034
- Ngày / CVAT local: 12/11/2026.
- Công cụ đã dùng: CVAT, pretrained model YOLO nano-seg, SegFormer

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh [LMS](https://aithucchien.vinuni.edu.vn) đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic | 3 / 3 | 20 |
| medium_instance | medium_instance | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic | 2 / 2 | 30 |
| cp1_holes | cp1_holes | 1 / 1 | 3 |
| cp2_slice | cp2_slice | 2 / 1 | 3 |
| cp5_occlusion | cp5_occlusion | 1 / 1 | 3 |
| cp3_thin | cp3_thin | 1 / 1 | 3 |
| cp4_curb | cp4_curb | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: `000000181542.jpg`, người mặc trang phục sáng màu ở giữa tiền cảnh (`person`). 
- Class và quy tắc tôi dùng để chọn biên: `person`; bám theo đường bao phần người nhìn thấy, không lấy xe máy, ô tô hay nền vào mask và không tự vẽ phần bị che.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Chưa xác định được một lỗi cụ thể chỉ từ thông tin có trong repo.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): repo không xác nhận đã chạy hoặc có metric liên quan; chưa có điểm. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| `cp1_holes` — `000000144300.jpg`, vùng kính/chi tiết bên trong xe | Kính là một phần của mask xe / kính là lỗ cần khoét | Quy tắc checkpoint: giữ kính/chi tiết trong mask; không khoét lỗ tùy tiện, chỉ phân biệt lỗ nền thật theo ảnh và quy tắc task | Giữ kính trong mask xe; nếu có vùng nền xuyên thật mà ảnh không đủ rõ, hỏi coach trước khi khoét |
| `cp2_slice` — `000000017627.jpg`, hai xe nằm sát nhau | Một mask cho vùng xe liền nhau / mỗi xe là một instance riêng | Đây là bài instance; hai vật cùng lớp sát nhau vẫn là hai instance | Tách thành hai mask `car`, mỗi mask theo biên phần xe nhìn thấy |
| `cp5_occlusion` — `000000336232.jpg`, phần xe bị vật khác che | Hai phần nhìn thấy là hai object / là các phần của cùng một object bị che | Quy tắc occlusion: một vật bị che vẫn là một instance; không vẽ xuyên vùng bị che | Giữ một instance cho các phần nhìn thấy của cùng xe, không nối mask qua vùng khuất; hỏi coach nếu không xác định được hai phần có cùng một vật hay không |

