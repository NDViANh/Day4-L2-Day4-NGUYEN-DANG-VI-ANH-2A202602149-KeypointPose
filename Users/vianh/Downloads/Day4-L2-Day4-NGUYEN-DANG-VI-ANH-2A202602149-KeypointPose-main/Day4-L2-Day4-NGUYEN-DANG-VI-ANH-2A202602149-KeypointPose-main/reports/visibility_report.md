# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 28 skeleton, trung bình 15.61 khớp có v > 0 mỗi người
- Tổng: v=2 334 | v=1 103 | v=0 39

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 6 | 0 | 21% |
| 1 | left_eye | 19 | 9 | 0 | 32% |
| 2 | right_eye | 20 | 8 | 0 | 29% |
| 3 | left_ear | 10 | 18 | 0 | 64% |
| 4 | right_ear | 15 | 13 | 0 | 46% |
| 5 | left_shoulder | 25 | 3 | 0 | 11% |
| 6 | right_shoulder | 28 | 0 | 0 | 0% |
| 7 | left_elbow | 23 | 3 | 2 | 11% |
| 8 | right_elbow | 25 | 3 | 0 | 11% |
| 9 | left_wrist | 20 | 5 | 3 | 18% |
| 10 | right_wrist | 20 | 7 | 1 | 25% |
| 11 | left_hip | 20 | 7 | 1 | 25% |
| 12 | right_hip | 21 | 6 | 1 | 21% |
| 13 | left_knee | 17 | 4 | 7 | 14% |
| 14 | right_knee | 18 | 4 | 6 | 14% |
| 15 | left_ankle | 16 | 3 | 9 | 11% |
| 16 | right_ankle | 15 | 4 | 9 | 14% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
