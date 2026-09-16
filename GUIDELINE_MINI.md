# Mini guideline - nhóm: T-019  |  người gán: Bùi Việt Nam  |  ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Ước lượng vị trí mấu chuyển lớn xương đùi (ngang nếp gấp đũng quần hoặc dưới thắt lưng khoảng 1/2 khoảng cách vai-gối). Đánh dấu `v=1` nếu mặc áo dài/quần thụng che khuất. | Hông hầu như không lộ trực tiếp dưới trang phục. Cần quy ước tỷ lệ giải phẫu cố định để tránh lệch 20-50 pixel giữa các annotator. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Nếu thấy một phần tai: chấm vào tâm tai (`v=2`). Nếu bị mũ bảo hiểm (full-face) hoặc tóc che kín hoàn toàn nhưng đầu vẫn trong ảnh: chấm vị trí giải phẫu ước lượng (ngang tầm mắt) và gắn `v=1`. | Che khuất bề mặt không làm mất cấu trúc hộp sọ; gán `v=1` giúp mô hình học liên hệ giữa hướng khuôn mặt và vị trí tai. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp nằm ngoài biên ảnh: gán cờ `v=0` (Outside) và toạ độ đưa về (0,0). Các khớp còn nằm trong ảnh: chấm đúng vị trí và gán `v=2` (hoặc `v=1` nếu bị che). | Mô hình không thể học dự đoán các điểm nằm ngoài ma trận pixel thực của ảnh. Tránh sinh toạ độ ảo ngoài khung. |
| Cổ tay nằm sau tay lái / sau thân mình | Ước lượng vị trí cổ tay dựa theo phương của cẳng tay và điểm khuỷu tay; gắn cờ `v=1` (Occluded). | Hướng của cẳng tay cung cấp ràng buộc hình học đủ tin cậy để ước lượng vị trí cổ tay với sai số chấp nhận được. |
| Hai người chồng lên nhau | Gán dứt điểm từng người một (vẽ trọn vẹn 17 điểm của người trước rồi mới sang người sau). Khớp của người sau bị che bởi người trước thì chấm ước lượng và gắn `v=1`. | Tránh lỗi nghiêm trọng "nhầm người" (kéo nhầm điểm của người này sang khung xương của người kia). |
| Người nhỏ đến mức nào thì không gán nữa | Gán toàn bộ người nhận diện được hình hài cơ thể (chiều cao bounding box > 30px). Không gán bóng người quá mờ hoặc tí hon ở hậu cảnh xa (< 20px). | Đảm bảo độ bao phủ 100% các đối tượng người chính trong 20 ảnh core của lab theo đúng rubric. |

*Ghi chú minh họa: Xem ảnh trực quan tại `outputs/vis_train/` (ví dụ `train_04.jpg` cho ca mũ bảo hiểm & tay lái mô tô; `train_16.jpg` cho ca quay lưng; `train_11.jpg` cho ca che khuất nửa thân dưới).*

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_04.jpg`, người thứ 2 (nữ lái mô tô bên phải), khớp `tai và chân dưới`

- **Mơ hồ ở chỗ nào**: Người lái mô tô đội mũ bảo hiểm che kín hoàn toàn tai, tóc. Nửa thân dưới (từ đùi trở xuống gồm gối và cổ chân) bị đầu xe máy và yếm xe che khuất.
- **Bạn quyết thế nào**: Khớp tai trái/phải đặt chấm ước lượng ngang đuôi mắt và gắn cờ `v=1` (Occluded). Các khớp chân dưới bị che khuất trong khung hình được gán cờ `v=1` hoặc `v=0` tương ứng theo mép khung hình.
- **Vì sao**: Cấu trúc mũ bảo hiểm ôm sát đầu cho phép định vị tâm tai chính xác trong khoảng 5-10px; không được để `v=0` cho tai khi toàn bộ đầu vẫn nằm gọn giữa ảnh.
- **Nếu người khác quyết ngược lại thì model học sai cái gì**: Nếu xoá khớp hoặc đặt `v=0`, model sẽ mất khả năng dự đoán pose người lái xe mô tô/xe máy khi có phương tiện che chắn.

### Ca 2 - ảnh `train_16.jpg`, người thứ 2 (cầu thủ áo trắng số 15 GER quay lưng), khớp `left/right shoulder & hip, eyes`

- **Mơ hồ ở chỗ nào**: Cầu thủ nhảy bật lên đón đĩa bay trong tư thế quay lưng hoàn toàn về phía camera và ngửa cổ nhìn lên trời. Góc nhìn từ phía sau rất dễ gây nhầm lẫn giữa bên trái và bên phải cơ thể.
- **Bạn quyết thế nào**: Xác định trái/phải nghiêm ngặt theo giải phẫu sinh học của vận động viên: khi quay lưng, tay/chân trái nằm ở bên trái bức ảnh, tay/chân phải nằm ở bên phải bức ảnh. Mắt trái bị che khuất gán cờ `v=1`.
- **Vì sao**: Nguyên tắc cốt lõi: Trái/phải tính theo cơ thể người, không theo chiều màn hình.
- **Nếu người khác quyết ngược lại thì model học sai cái gì**: Model sẽ bị lỗi đảo trái/phải (`dao_trai_phai`). Đây là lỗi nguy hiểm nhất vì data augmentation lật ảnh ngang (horizontal flip) sẽ nhân đôi sai số khiến model suy luận sai hướng chuyển động vĩnh viễn.

### Ca 3 - ảnh `train_11.jpg`, người thứ 1 (cô gái ngồi sau bàn ăn), khớp `lower body (knee, ankle)`

- **Mơ hồ ở chỗ nào**: Cô gái ngồi sau bàn gỗ ngoài trời, trên bàn có con mèo và hộp pizza che khuất toàn bộ nửa thân dưới từ thắt lưng trở xuống.
- **Bạn quyết thế nào**: Thân trên gán đầy đủ (`v=2`), các khớp cổ tay và hông bị che một phần gán `v=1`. Phần đầu gối và cổ chân bị mặt bàn gỗ che hoàn toàn, xác định mép ảnh/mặt bàn để gán cờ che khuất hoặc ra ngoài biên.
- **Vì sao**: Mặt bàn là vật che chắn cứng, vị trí khớp hông vẫn có thể suy đoán từ cột sống và vai, nhưng khớp chân hoàn toàn không có tín hiệu pixel trực tiếp.
- **Nếu người khác quyết ngược lại thì model học sai cái gì**: Nếu chấm bừa toạ độ chân đè lên mặt bàn/hộp pizza, model sẽ học sai rằng chân người có thể nằm đè lên mặt bàn khi ngồi.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_ear` (48%) và `right_ear` (41%), kế tiếp là `left_hip` (31%) và `left_wrist` / `right_wrist` (28%).
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Chủ yếu do **guideline về che khuất tai (tóc/mũ) và hông chưa thống nhất chi tiết**: một bên thấy tóc phủ nhẹ là vội đánh `v=1`, bên kia thì vẫn cố đánh `v=2` vào vùng tóc; tương tự với khớp hông khi mặc áo dài rộng.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: 
  1. Nếu tai bị tóc hoặc mũ che > 30% diện tích tai: bắt buộc đánh dấu `v=1` và chấm vào vị trí giải phẫu ước lượng.
  2. Với khớp hông mặc quần áo thụng/áo khoác: luôn đánh dấu `v=1` và lấy vị trí dựa trên tỷ lệ chuẩn 1/2 vai - gối.
