# Báo cáo Day 3 — Video Tracking

- **Họ và tên**: Trần Đăng Khang
- **MSSV**: 2A202602189
- **Hình thức**: Cá nhân

## 1. Bảng tổng hợp kết quả (Metrics Table)

| Cấu hình / Phép so | HOTA | DetA | AssA | LocA | IDF1 | MOTA | MOTP | FP | FN | IDSW |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Bạn vs Gold** | 0.803 | 0.784 | 0.830 | 0.852 | 0.969 | 0.939 | 0.835 | 15 | 20 | 0 |
| **ByteTrack vs Gold** | 0.709 | 0.649 | 0.776 | 0.846 | 0.875 | 0.749 | 0.823 | 88 | 54 | 2 |
| **ReID vs Gold** | 0.763 | 0.711 | 0.820 | 0.872 | 0.900 | 0.792 | 0.860 | 91 | 26 | 2 |
| **ReID vs Bạn** | 0.746 | 0.683 | 0.822 | 0.861 | 0.889 | 0.764 | 0.846 | 102 | 32 | 0 |

## 2. Trả lời câu hỏi phân tích

### Câu 1: MOTA của bạn cao hơn hay thấp hơn IDF1? Vì sao MOTA không phạt nặng lỗi ID?
- **Kết quả**: MOTA của tôi là `0.939`, thấp hơn một chút so với IDF1 là `0.969`. Cả hai chỉ số đều đạt mức rất cao chứng tỏ nhãn gán tay chính xác tốt.
- **Giải thích**: MOTA tính toán dựa trên tổng số lỗi tức thời (FP + FN + IDSW) chia cho tổng số hộp thực tế. Vì IDSW chỉ xảy ra tại đúng frame chuyển giao nhãn sai (được tính là 1 lỗi duy nhất tại frame đó), nên nếu một track bị đổi ID một lần duy nhất rồi đi tiếp chính xác, MOTA chỉ phạt 1 điểm. Ngược lại, IDF1 đánh giá sự nhất quán toàn cục xuyên suốt hành trình (identity matching). Nếu ID bị đổi, nó sẽ tính phạt trên toàn bộ chiều dài nửa sau của track đó vì không khớp với ID toàn cục đã ánh xạ, do đó IDF1 phản ánh độ nhạy với ID chặt chẽ hơn nhiều so với MOTA.

### Câu 2: ByteTrack control và BoT-SORT + ReID khác nhau thế nào ở IDF1, AssA, IDSW?
- **So sánh**: 
  - **BoT-SORT + ReID** (Treatment) cho kết quả vượt trội hơn so với **ByteTrack** (Control) trên tất cả các metric tracking quan trọng: HOTA tăng từ `0.709` lên `0.763`, AssA tăng từ `0.776` lên `0.820`, IDF1 tăng đáng kể từ `0.875` lên `0.900`.
  - Cả hai cấu hình đều xuất hiện 2 lỗi ID Switch (`IDSW = 2`). Tuy nhiên, nhờ sự bổ trợ thông tin đặc trưng ngoại quan (Appearance Features) của ReID, quá trình liên kết các hộp (association) có độ tin cậy tốt hơn ở các phân đoạn đối tượng bị mờ, rung lắc hoặc che khuất tạm thời, giúp tăng điểm chất lượng liên kết AssA lên đáng kể.

### Câu 3: DetA, FP và FN thay đổi thế nào? Lỗi còn lại là do detector hay association?
- **Phân tích**: 
  - Khi chuyển từ ByteTrack sang ReID, DetA tăng mạnh từ `0.649` lên `0.711`.
  - Số lượng lỗi bỏ sót (FN) giảm đi một nửa: từ `54` xuống còn `26`.
  - Lượng lỗi gán thừa (FP) tăng nhẹ từ `88` lên `91`.
  - Lỗi còn lại chủ yếu đến từ **Detector**. Việc hạ thấp ngưỡng tự tin hoặc thay đổi thuật toán tracker giúp thu hồi được nhiều hộp mờ nhạt (giảm FN) nhưng đồng thời cũng kéo theo một số lượng nhiễu nền bị gán nhãn sai thành ô tô (FP tương đối cao ~91).

### Câu 4: Phân tích các trường hợp bất đồng thực tế
- **Một chỗ tôi gán đúng nhưng ReID gán sai**: Tại **Frame 107**, ReID bắt nhầm các vật thể tĩnh ở rìa ngoài lề đường (như biển quảng cáo hoặc chướng ngại vật đứng yên) và gán ID 7 (kéo dài tới 43 frames). Đây là lỗi FP điển hình của detector khi nhận diện nhầm tĩnh vật.
- **Một chỗ ReID gán đúng mà tôi cần xem lại**: Tại **Frame 170**, model ReID phát hiện đầy đủ xe đi vào góc tối mờ ở bên phải trong khi tôi gán nhãn hơi trễ hoặc bị sót bbox ở frame rìa này do độ tương phản quá kém.

### Câu 5: Cải tiến GUIDELINE_MINI.md cho các dự án lớn hơn
- Sẽ bổ sung rõ quy định về vùng loại trừ (Ignore Zone/Don't care): Không gán các vật thể tĩnh đỗ ở bãi xe trong góc tối hoặc bảng quảng cáo.
- Quy định cụ thể ngưỡng kích thước pixel tối thiểu (ví dụ: chiều rộng xe < 15 pixel) để quyết định dừng track sớm một cách nhất quán, tránh việc người này gán người kia bỏ qua ở những khoảng cách quá xa.
