# Guideline gán nhãn Tracker cá nhân

## 1. Định nghĩa đối tượng xe mục tiêu

| Nên gán (Target Bbox) | KHÔNG gán (Ignore Bbox) |
| --- | --- |
| Xe ô tô con, Sedan, SUV | Xe máy, xe đạp, người đi bộ |
| Xe buýt, xe khách nhỏ | Xe đồ chơi, xe in trên biển quảng cáo |
| Xe tải, xe container | Xe mô tô ba bánh |

## 2. Luật ID

| Tình huống | Luật áp dụng | Vì sao |
| --- | --- | --- |
| Xe bị che một phần rồi hiện lại | Giữ nguyên ID nếu bị che dưới 25 frame | Đảm bảo tính liên tục của hành trình (identity) |
| Xe bị che lâu hơn 25 frame | Đánh ID mới | Tránh nhập nhằng liên kết sau thời gian quá dài |
| Xe rời khung hình rồi quay lại | Đánh ID mới | Theo chuẩn MOT, đối tượng quay lại là track mới |
| Hai xe cắt nhau | Giữ đúng ID riêng của từng xe | ReID và Kalman Filter hỗ trợ phân tách |

## 3. Luật bbox

- Xe bị cắt bởi rìa ảnh: Bbox chạm sát rìa ảnh, không tự vẽ đoán phần ẩn.
- Xe bị xe khác che khuất một phần: Chỉ vẽ ôm sát phần nhìn thấy được để tránh nhiễu đặc trưng.
- Xe đỗ tĩnh dài ngày: Vẫn giữ nguyên bbox và ID nhất quán xuyên suốt video.

## 4. Các ca mơ hồ thực tế đã xử lý
- **Ca 1 (Frame 80-92, ID 5)**: Xe bị che khuất một phần bởi cây cối rìa đường. Quyết định: Thêm keyframe dày hơn (mỗi 3 frames) để bbox không bị lệch.
- **Ca 2 (Frame 107, ID 7)**: Model bắt nhầm xe nằm trên biển hiệu quảng cáo tĩnh. Quyết định: Bỏ qua không gán nhãn vì đó không phải xe tham gia giao thông.
- **Ca 3 (Frame 170, ID 8)**: Xe đi vào vùng tối mờ ở rìa phải. Quyết định: Giữ nguyên ID và bám sát viền mờ của xe.
