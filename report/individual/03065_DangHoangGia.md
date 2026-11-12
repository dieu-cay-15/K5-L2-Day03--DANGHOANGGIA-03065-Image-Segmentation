# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202603065 (Đặng Hoàng Gia — K5-L2A)
- Ngày / CVAT local: 11/11/2026 – 12/11/2026 / CVAT (`app.cvat.ai`)
- Công cụ đã dùng: CVAT Polygon, Brush/Eraser, `scripts/inspect_submissions.py` và gợi ý tự động (Mask R-CNN) có kiểm sửa lại

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh [LMS](https://aithucchien.vinuni.edu.vn) đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | `submissions/easy_semantic.zip` | 3 / 3 | 20 |
| medium_instance | `submissions/medium_instance.zip` | 3 / 3 | 32 |
| hard_panoptic | `submissions/hard_panoptic.zip` | 2 / 2 | 30 |
| cp1_holes | `submissions/cp1_holes.zip` | 1 / 1 | 3 |
| cp2_slice | `submissions/cp2_slice.zip` | 1 / 1 | 3 |
| cp5_occlusion | `submissions/cp5_occlusion.zip` | 1 / 1 | 3 |
| cp3_thin | `submissions/cp3_thin.zip` | 1 / 1 | 3 |
| cp4_curb | `submissions/cp4_curb.zip` | 1 / 1 | 3 |
| cp6_coverage | `submissions/cp6_coverage.zip` | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg`, object `#1` là phần đầu chiếc xe máy góc dưới bên trái (`bbox = [0, 504, 63, 95]`) và object `#2` là người phụ nữ mặc áo dài đi bộ qua vạch kẻ đường ở giữa ảnh (`bbox = [186, 132, 118, 329]`).
- Class và quy tắc tôi dùng để chọn biên: Gán `#1` là `motorcycle` và `#2` là `person`; dùng công cụ Polygon bám sát phần thân và tà áo dài thực sự nhìn thấy của người đi bộ, dừng biên đúng tại mép tà áo để tách biệt với phần thân chiếc xe máy chạy ngang phía sau.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Gợi ý tự động nhận diện tốt các người lái xe lớn ở tiền cảnh nhưng dễ gộp hai người đi bộ đứng sát nhau hoặc bỏ sót xe đỗ ở xa; tôi giữ các mask đúng biên và tự chỉnh/vẽ bổ sung các xe đỗ sát nhau ở `000000373353.jpg` và `000000458325.jpg`.
- Nếu không dùng gợi ý: ghi “không dùng”; vẫn giải thích một quyết định gán nhãn của mình.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: `medium_instance` / ảnh `000000181542.jpg` (bánh xe góc dưới trái `bbox = [0, 504, 63, 95]` và xe bán tải màu bạc ở trung cảnh `bbox = [269, 117, 241, 127]`) cùng các xe/người ở nền xa trên `000000373353.jpg` và `000000458325.jpg`.
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: sai lớp và thiếu-thừa vật (nhầm `bicycle` thành `motorcycle`, nhầm `truck` bán tải thành `car`, và bỏ sót một số vật nhỏ ở xa).
- Bằng chứng tôi nhìn thấy: Khi kiểm tra lại ảnh `000000181542.jpg` và đối chiếu kết quả tự chấm (`scoring/score.py`), bánh xe góc dưới trái có nan hoa mảnh của xe đạp (`bicycle`), chiếc xe bạc ở giữa có thùng chở hàng phía sau (`truck`), và `recall` ban đầu chỉ đạt `R@0.5 = 0.66` (`FN = 24`).
- Quy tắc và hành động sửa: Đổi nhãn object `[0, 504, 63, 95]` sang `bicycle`, đổi xe bán tải `[269, 117, 241, 127]` sang `truck`, đồng thời bổ sung các polygon vật bị sót ở xa mà không chồng lấn với các mask đã vẽ.
- Sau sửa đã Save và export lại chưa? Đã cập nhật, xuất lại `COCO 1.0` thành `submissions/medium_instance.zip` và kiểm tra bằng `scripts/inspect_submissions.py` đạt `[OK]`.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): Đã chạy `scoring/scorecard.py --group tiers --dir submissions`: `medium_instance` tăng từ metric `0.513` (`8.0 / 32`, `TP = 47, FN = 24`) lên `0.647` (`17.6 / 32`, `TP = 60, FN = 11, R@0.5 = 0.85`); `easy_semantic` tăng từ `0.802` (`17.9 / 20`) lên `0.804` (`18.0 / 20`); `hard_panoptic` đạt `PQ = 0.417` (`14.5 / 30`); tổng 3 tier đạt `50.1 / 82` (không có `review signals`). Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `easy_semantic` — `817bca71-00000000.jpg` (vùng thùng rác `x = 302..395, y = 448..516`, hộp điện `x = 830..960, y = 388..516` và ranh bó vỉa `x = 548..1000, y = 440..558`) | (A) Tô tràn `road`/`sidewalk`/`vegetation` đè lên cả thùng rác, hộp điện và xe đỗ; hoặc (B) Đưa gờ bó vỉa vào `sidewalk` và để trống (`background`) các vật không thuộc 5 class của task. | `classes.json` của `easy_semantic` chỉ có 5 lớp (`road`, `sidewalk`, `building`, `vegetation`, `sky`); bó vỉa thuộc về `sidewalk` theo chức năng. | Chọn (B): gờ bó vỉa gán `sidewalk`, còn thùng rác, hộp điện và xe ô tô để trống (`background`). |
| 2. `medium_instance` — `000000373353.jpg` (xe buýt 2 tầng `bbox = [269, 215, 98, 141]` phía sau taxi vàng `bbox = [272, 313, 98, 75]` và người đi bộ `bbox = [292, 297, 60, 118]`) | (A) Vẽ mask `bus` kéo xuống bao trùm cả phần bị taxi vàng che và vẽ mask taxi đè lên người đi bộ; hoặc (B) Cắt biên mỗi vật dừng đúng tại ranh giới phần thực sự nhìn thấy. | Quy tắc trong `README.md`: chỉ vẽ phần nhìn thấy, không tự đoán biên sau vật che khuất. | Chọn (B): tách riêng `person`, `car` (taxi) và `bus` chỉ theo phần diện tích nhìn thấy không chồng lấn. |
| 3. `cp1_holes` (`000000144300.jpg`, ô cửa kính xe caravan trắng `x = 15..118, y = 12..72`) và `cp5_occlusion` (`000000336232.jpg`, ô tô tối màu `x = 85..198, y = 123..184` sau người lái xe máy) | (A) Khoét lỗ vùng cửa kính xe ở `cp1_holes` và tách chiếc xe bị người che ở `cp5_occlusion` thành 2 object; hoặc (B) Giữ cửa kính nằm trong mask xe và gộp hai nửa xe bị che thành 1 instance duy nhất. | `CHECKPOINTS.md`: cửa kính nằm trong mask của xe (không khoét tùy tiện); vật bị che làm đôi vẫn là một instance duy nhất. | Chọn (B): giữ nguyên cửa kính trong mask ở `cp1_holes` và giữ 1 instance đa polygon cho chiếc `car` bị che ở `cp5_occlusion`. |
