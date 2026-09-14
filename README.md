# Day 3 — Video Tracking Data Lab

Trong 4 giờ, bạn sẽ gán nhãn tracking bằng CVAT Track Mode, tự kiểm ba lượt,
kiểm chéo, khóa bản annotation độc lập, evaluate/rework với teaching reference,
rồi chạy hai cấu hình trên chính clip đó: YOLO26n + ByteTrack làm control và
YOLO26n + BoT-SORT + ReID làm treatment. Mục tiêu là hiểu association/identity,
không phải tin model là đáp án.

## Bắt đầu

1. Mở [GUIDE.md](GUIDE.md) hoặc `lab-guide.html` và làm theo timeline.
2. Đọc [CVAT_TASK_SPEC.md](CVAT_TASK_SPEC.md) trước khi tạo task.
3. Dùng [GUIDELINE_MINI.md](GUIDELINE_MINI.md) trong lúc ra quyết định.
4. Đối chiếu [RUBRIC.md](RUBRIC.md) và mẫu trong `reports/` trước khi nộp.
5. Đọc [ReID theory](docs/day3-reid-theory.md) ngay trước phần notebook.

## Ba nguyên tắc không được đảo thứ tự

- Gán nhãn độc lập trước khi xem teaching reference hoặc model.
- Chạy `python3 tools/lock_pre_gold.py` và gửi hash cho Lab Coach trước khi nhận
  `gold/clip_01/gt.txt`.
- Model output là baseline để chẩn đoán, không phải đáp án.

Core path không yêu cầu viết code: copy/paste các lệnh trong GUIDE và chạy các
cell notebook theo thứ tự. Nếu gặp lỗi, ghi lại thông báo đầy đủ và báo Lab Coach;
không tự sửa tay file MOT hoặc đổi schema.
