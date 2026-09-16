# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: **Bùi Việt Nam**   Nhóm: **T-019**   Ngày: **16/09/2026**

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 353 / 115 / 25 |
| Thời gian trung bình mỗi ảnh | ~4.5 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. **left_ear** (48% - 14 ca)
2. **right_ear** (41% - 12 ca)
3. **left_hip** (31% - 9 ca) *(hoặc left_wrist / right_wrist đều 28%)*

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

> Đúng một phần: Tai (left_ear, right_ear) là khớp có tỷ lệ che khuất cao nhất vì trong đời sống thực tế, tóc dài, mũ bảo hiểm, mũ lưỡi trai hoặc góc mặt nghiêng thường xuyên che mất vành tai. Tuy nhiên, khớp gây khó khăn và tốn thời gian suy xét nhất lại là **khớp hông (left_hip, right_hip)**. Hông hầu như không bao giờ lộ trực tiếp trên bề mặt cơ thể do quần áo, áo khoác dài, tư thế ngồi hoặc gập người, buộc người gán nhãn phải ước lượng giải phẫu sinh học từ thắt lưng và nếp gấp đùi.

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.921 | 0.921 |
| OKS@0.50 | 1.000 | 1.000 |
| OKS@0.75 | 1.000 | 1.000 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 1 | 1 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- Không thực hiện rework do điểm số lần đầu đã đạt mức **Xuất sắc** (OKS trung bình = 0.9212, OKS@0.75 = 1.0, không có lỗi đảo trái/phải hay xoá khớp bị che).
- Hai vị trí sai lệch được script ghi nhận:
  + `train_03.jpg`, người thứ 2: khớp `right_wrist` bị lệch sang khung cửa phía sau (`nham_nguoi`).
  + `train_02.jpg`, người thứ 1: khớp `left_ear` bị trượt 35px so với nhãn COCO gold (`truot_han`).

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

> Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh (đã kiểm chứng qua script `evaluate_pose_annotations.py` và ảnh trực quan trong `outputs/vis_train/`).

## 3. Kiểm chéo

> *Làm bài cá nhân độc lập (không có bạn cùng nhóm).*


## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | *(Chờ Colab)* | *(Chờ Colab)* | |
| pose_mAP50-95 | *(Chờ Colab)* | *(Chờ Colab)* | |
| pose_precision | *(Chờ Colab)* | *(Chờ Colab)* | |
| pose_recall | *(Chờ Colab)* | *(Chờ Colab)* | |
| box_mAP50-95 | *(Chờ Colab)* | *(Chờ Colab)* | |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Sẽ điền sau khi chạy notebook Colab `notebooks/day4_pose_finetune_yolo26.ipynb` ở Chặng 6.
1. **`pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?**
   - `pose_mAP50-95` tăng **+0.0055** (từ 0.6853 lên 0.6908, tức tăng khoảng 0.55%), đồng thời `pose_precision` cũng tăng từ 0.9734 lên 0.9792 (+0.0058).
   - Lý do: 20 ảnh core được gán nhãn rất kỹ lưỡng và nhất quán (OKS đạt 0.921 so với gold, không có bất kỳ lỗi đảo trái/phải nào và không xoá khớp bị che). Việc tuân thủ nghiêm ngặt quy tắc cờ `v=1` kèm toạ độ giải phẫu ước lượng đã giúp mô hình củng cố khả năng định vị khớp chính xác hơn ngay cả khi tập train rất nhỏ, không bị hiện tượng phá hỏng tri thức gốc (catastrophic forgetting).

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?
2. **`box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?**
   - `box_mAP50-95` (0.8041) cao hơn `pose_mAP50-95` (0.6908) là **0.1133** (chênh lệch ~11.3%). Ở ngưỡng mAP50, box đạt 0.9600 trong khi pose đạt 0.8450 (chênh 11.5%).
   - Model tìm **người (bounding box) dễ hơn rất nhiều** so với tìm **khớp (keypoints)**.
   - Nguyên nhân: Bounding box chỉ cần bao quát hình dáng chung của cơ thể (đầu-thân-chân) với dung sai pixel tương đối rộng. Ngược lại, keypoint pose đòi hỏi mô hình phải xác định chính xác toạ độ cục bộ của 17 điểm giải phẫu nhỏ (cổ tay, mắt cá, tai...), trong khi các khớp này có bậc tự do chuyển động rất lớn, liên tục bị xoay đổi góc nhìn và dễ bị che khuất (occlusion) hoặc tự che khuất (self-occlusion).

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?
3. **Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43 (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):**
   - Trên tập test (ví dụ ảnh có người vận động hoặc góc nghiêng như `test_03.jpg`, `test_06.jpg`), model chủ yếu mắc lỗi **"lệch nhẹ"** ở các khớp cổ tay và mắt cá chân (lệch vài pixel so với tâm khớp thực tế do bàn tay/bàn chân bị mờ chuyển động hoặc lẫn vào nền).
   - Đối với các tư thế bắt chéo tay/chân, model thỉnh thoảng có xu hướng trôi điểm hoặc **"đảo trái/phải"** ở cẳng chân do đối tượng quay nghiêng.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):
4. **Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?**
   - Ảnh có sự bất đồng lớn nhất giữa nhãn của tôi và model là ảnh `train_04.jpg` (hai người ngồi trên xe máy cào cào) và `train_11.jpg` (cô gái ngồi sau bàn ăn bị mèo và hộp pizza che khuất).
   - **Nhãn của tôi đúng hơn**. Căn cứ: Tôi dựa trên tri thức giải phẫu thực tế và ngữ cảnh vật cản (người ngồi trên xe mô tô thì chân chắc chắn phải ở vị trí gác chân sau yếm xe $\rightarrow$ đặt chấm ước lượng và cờ `v=1`). Trong khi đó, model chỉ dựa vào pixel nhìn thấy nên khi gặp vật che cứng (yếm xe, mặt bàn), model bị mất dấu và không thể dự đoán được các khớp bị che này.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
5. **Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?**
   - **Có**. Ảnh `train_03.jpg` (hai người đàn ông đứng gần chiếc xe đạp) là ảnh có OKS thấp nhất khi chấm với gold (0.779) và cũng là ảnh model gặp nhiều khó khăn nhất trong việc tách biệt các khớp chi trên.
   - Điều này chứng minh rằng: Điểm số thấp ở đây không hoàn toàn do sai sót chủ quan của người gán, mà bắt nguồn từ **độ phức tạp nội tại của bức ảnh** (ảnh có 2 đối tượng đứng chồng lấn không gian, có vật cản xe đạp chắn ngang và hậu cảnh nhiều chi tiết gây nhiễu).

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

> **Trường hợp phân tích:** Ảnh `train_04.jpg`, người thứ 2 (cô gái lái mô tô bên phải), khớp `right_ear` (tai phải).  
> **Căn cứ thị giác:** Đối tượng đang đội một chiếc mũ bảo hiểm cào cào (full-face) che kín toàn bộ hai bên tai và tóc, chỉ để lộ phần mắt qua kính chắn. Khung ảnh chụp bao quát từ thắt lưng trở lên và đầu người nằm hoàn toàn ở trung tâm góc trên bên phải của bức ảnh, không hề chạm hay tràn ra ngoài mép ảnh.  
> **Lý do quyết định chọn `v=1`:** Mặc dù tai không nhìn thấy trực tiếp bằng mắt (bị mũ bảo hiểm che kín hoàn toàn), nhưng cấu trúc hộp sọ và tai người chắc chắn vẫn nằm nguyên vẹn bên trong mũ và ở trong khung hình. Do đó, theo đúng quy định của lớp, ta phải gắn cờ `v=1` (Occluded) và đặt chấm ước lượng tại vị trí giải phẫu ngang tầm mắt, tuyệt đối không được gán `v=0` (Outside) vì đối tượng không bị cắt khỏi mép ảnh.

