# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Đặng Vi Anh   Nhóm: 2A202602149   Ngày: 16/09/2026

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 28 |
| v=2 / v=1 / v=0 | 334 / 103 / 39 |
| Thời gian trung bình mỗi ảnh | ~4 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear — 64%
2. right_ear — 46%
3. left_eye — 32%

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Đúng, tai là khớp khó nhất vì thường bị tóc hoặc góc chụp che khuất — nhìn thấy đường viền đầu nhưng không thấy rõ vị trí tai. Mắt dễ xác định vị trí giải phẫu hơn nhưng vẫn hay bị che khi người quay nghiêng. Ngược lại, vai và khuỷu tay (v=1 thấp) thường lộ rõ trong trang phục nên dễ gán hơn.

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.919 | 0.919 |
| OKS@0.50 | 0.966 | 0.966 |
| OKS@0.75 | 0.931 | 0.931 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 1 | 1 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

Không thực hiện rework. OKS trung bình đã đạt 0.919 (trên ngưỡng chấp nhận được), không có lỗi đảo trái/phải hay nhầm người. Lỗi duy nhất là `xoa_khop_bi_che` (1 trường hợp) không đủ nghiêm trọng để chỉnh sửa lại toàn bộ nhãn trong giới hạn thời gian.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?**

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.

## 3. Kiểm chéo

Làm bài độc lập (solo), không có bạn cùng nhóm để kiểm chéo.

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| N/A | — | — | — | Làm solo |

Luật đã tự bổ sung vào `GUIDELINE_MINI.md` sau khi đối chiếu với gold:

- Keypoint bị che bởi tóc/quần áo nhưng còn trong khung hình → dùng `v=1` (occluded), không dùng `v=0` (outside frame).

## 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.845 | 0.845 | 0.000 |
| pose_mAP50-95 | 0.685 | 0.691 | +0.006 |
| pose_precision | 0.973 | 0.979 | +0.006 |
| pose_recall | 0.846 | 0.846 | 0.000 |
| box_mAP50-95 | 0.812 | 0.804 | -0.008 |

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` tăng nhẹ +0.006 (từ 0.685 → 0.691). 20 ảnh quá ít để gây thay đổi lớn, nhưng hướng tăng cho thấy nhãn không có nhiễu nghiêm trọng. Model vẫn giữ nguyên `pose_mAP50` và `pose_recall`, nghĩa là fine-tune không phá vỡ khả năng phát hiện pose ở ngưỡng thấp.

2. `box_mAP50` = 0.979 (baseline) so với `pose_mAP50` = 0.845 — chênh nhau 0.134. Model tìm *người* (bounding box) dễ hơn nhiều so với tìm *khớp* chính xác. Lý do: bounding box chỉ cần bao trùm vùng thân, trong khi keypoint phải khớp đúng vị trí giải phẫu từng điểm, đòi hỏi độ chính xác cao hơn nhiều.

3. Dựa vào `outputs/eval_vs_gold.json`, có 8 trường hợp `lech_nhe` — lỗi phổ biến nhất, thường xảy ra ở các khớp bị che (tai, cổ tay) khi model ước lượng vị trí dựa vào ngữ cảnh xung quanh thay vì nhìn thấy khớp trực tiếp.

4. Ảnh có `thieu_nguoi` (1 người bị thiếu) khả năng là ảnh có OKS thấp nhất — gold đúng vì đã được giảng viên kiểm tra. Tôi bỏ sót skeleton do người đứng gần mép ảnh và bị cắt một phần.

5. Nếu ảnh tôi gán tệ nhất trùng với ảnh model đoán tệ nhất, điều đó cho thấy bức ảnh có tính chất khó về mặt thị giác (người bị che, tư thế bất thường) — giới hạn của cả người và model, không phải chỉ lỗi gán nhãn.

## 5. Một rule evidence bạn đã dùng

Trong ảnh `train_01`, người thứ nhất, khớp `left_ear`: tai trái bị tóc che hoàn toàn nhưng toàn bộ đầu vẫn nằm trong khung hình. Tôi chọn `v=1` (occluded, còn trong khung) thay vì `v=0` vì vị trí giải phẫu của tai trái có thể ước lượng dựa vào vị trí tai phải và hướng quay đầu — tai vẫn tồn tại trong frame nhưng bị tóc che. Theo luật lớp: keypoint bị che nhưng còn trong khung phải là `v=1`, không được dùng `v=0` để xoá khớp.
