# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên:Nguyễn Trường Nam
- MSSV:2A202604067
- Email:truongnam0304@gmail.com
- Lớp:
- Link repo bài làm: https://github.com/ntnguyen03/K5-L2-DAY02-Nguy-n-Tr-ng-Nam-2A202604067-ObjectDetection.git
- Link Colab đã chạy:https://colab.research.google.com/drive/189dny6yLRgBhAbq65AkvssmTF8TC7QXF?usp=sharing

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.
Schema cố định: 0 car (sedan, SUV, taxi, bán tải), 1 truck (xe tải, đầu kéo), 2 bus (xe buýt, xe khách, minibus chở khách), 3 van (van/minivan chở người hoặc hàng). Xe đỗ vẫn cần gán nhãn; mỗi phương tiện một bounding box quanh phần thân nhìn thấy, không suy đoán phần nằm ngoài khung hoặc bị che khuất.

Ba tình huống đã ghi trong guideline cá nhân:

| Ảnh | Ca mơ hồ | Quyết định và căn cứ |
|---|---|---|
| `drive_003.jpg` | Xe ở xa, kích thước nhỏ, hình mờ | Chỉ gán khi còn đủ đặc điểm xác định lớp; không đoán lớp của xe không thể nhận diện. |
| `drive_007.jpg` | Xe bị vật cản/phương tiện khác che một phần | Nếu nhận diện được ô tô con, gán `car`; box chỉ bao phần thân nhìn thấy. |
| `drive_009.jpg` | Xe đỗ ven đường, một số xe có hình dáng dễ nhầm `car`/`truck`/`van` | Xe đỗ vẫn gán; bán tải thuộc `car`, tải chở hàng thuộc `truck`, van/minivan thuộc `van`. |

Phần cần thống nhất với giảng viên: ranh giới những xe quá nhỏ/mờ nên bỏ qua, và trường hợp van/minibus khó nhận diện khi bị che khuất. Trường hợp mơ hồ không tự thay đổi schema.

## 3. Annotation và self-QC

- **Dữ liệu:** 50 ảnh train (`drive_001.jpg`–`drive_050.jpg`) và 50 file nhãn YOLO tương ứng trong `dataset/labels/train/`.
- **Công cụ:** CVAT Community chạy local qua Docker/WSL
- **Định dạng:** Mỗi dòng nhãn gồm `class_id x_center y_center width height`, tọa độ chuẩn hóa, file `.txt` có cùng basename với ảnh. Giữ nguyên train/eval split.
- **Kiểm tra cấu trúc:** Chạy `python tools/check_yolo_labels.py` tại thư mục gốc repo, thu được:

```text
ĐẠT cấu trúc: 50/50 file nhãn hợp lệ. Chờ ground truth để chấm chất lượng.
```

- **Self-QC thủ công:** Đã rà ảnh và chỉnh các đối tượng trong CVAT. Danh sách 10 ảnh để xác nhận trong nhật ký rà ảnh: `drive_001.jpg`, `drive_002.jpg`, `drive_003.jpg`, `drive_004.jpg`, `drive_005.jpg`, `drive_006.jpg`, `drive_007.jpg`, `drive_008.jpg`, `drive_009.jpg`, `drive_010.jpg`
- **Commit chứa bản nhãn được Colab clone:** `ea7829cb9129082e91a6973c5360512af0b7a254`
## 4. Gold QC và rework

| Chỉ số | Kết quả ban đầu | Sau rework |
|---|---:|---|
| True positive (TP) | **399** | *Chưa có kết quả* |
| False positive (FP) | **321** | *Chưa có kết quả* |
| False negative (FN) | **254** | *Chưa có kết quả* |
| Precision | **0.5542** (55,42%) | *Chưa có kết quả* |
| Recall | **0.6110** (61,10%) | *Chưa có kết quả* |
| F1 @ IoU 0.50 | **0.5812** (58,12%) | *Chưa có kết quả* |
| mean_iou_of_matches | **0.8573** (85,73%) | *Chưa có kết quả* |
| `passed` | **false** | *Chưa có kết quả* |
| Ngưỡng bắt buộc | F1 ≥ 0.80 **và** Mean IoU ≥ 0.70 | |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: 
    - Google Colab GPU **NVIDIA Tesla T4** (14.9 GB VRAM hiển thị), CUDA:0. Ultralytics **8.4.208**, PyTorch **2.14.1+cu130**.
- Các tham số thay đổi so với notebook nếu có và lý do:
    - `epochs=50`, `imgsz=640`, `batch=-1` (AutoBatch đã chọn **47**), `seed=20260909`, `device=0`; optimizer tự chọn **AdamW** (`lr=0.00125`).  Không có bằng chứng thay đổi tham số so với notebook được cung cấp
- Đường dẫn best checkpoint trong phiên chạy:
    - `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt`

| Chỉ số trên 10 ảnh eval | Giá trị | Tỷ lệ |
|---|---:|---:|
| Precision | **0.410106** | **41.01%** |
| Recall | **0.229817** | **22.98%** |
| mAP50 | **0.271934** | **27.19%** |
| mAP50–95 | **0.201551** | **20.16%** |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
    - `bus` có recall thấp nhất, **0.000**, trên 2 đối tượng gold thuộc lớp này. `drive_053.jpg` là ví dụ có thể quan sát: xe buýt màu xanh ở tiền cảnh lại có box `van 0.85`. Vì dự đoán hình ảnh và metrics dùng những tiêu chí khác nhau, cần so trực tiếp với gold trước khi kết luận chính xác cặp box nào bị tính FN.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
    - Kiểm độ bám sát bốn cạnh box, box rộng/hẹp quá mức, lệch tâm, cắt rìa, che khuất và tính nhất quán giữa các annotator/ảnh. mAP50 xét IoU 0.50, trong khi mAP50–95 trung bình nhiều ngưỡng khắt khe hơn; box chỉ chồng lấp vừa đủ có thể đạt mAP50 nhưng không đạt nhiều ngưỡng cao. Trong **kết quả thực tế của tôi**, cả mAP50 **0.272** và mAP50–95 **0.202** đều thấp; đây không phải ví dụ mAP50 “cao”.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
    - Dấu hiệu rõ nhất là nhầm `car` ↔ `van` (`drive_051`, `drive_054`) và `bus` ↔ `van` (`drive_053`). Guideline cần nhấn mạnh rằng sedan/SUV/xe bán tải thuộc `car`, xe buýt chở khách thuộc `bus`, còn van/minivan thuộc `van` theo kiểu thân và chức năng; không phân biệt chỉ bằng kích thước hoặc confidence model. Metrics cũng hỗ trợ hướng rà: recall `bus=0`, `van≈0.0833`, trong khi `car≈0.636`. Chưa khẳng định đây là nguyên nhân duy nhất của kết quả thấp.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
    - Đưa ảnh eval vào train sẽ gây rò rỉ dữ liệu và làm đánh giá quá lạc quan. Trong notebook, tập 10 ảnh eval là tập **validation** dùng theo dõi hiệu suất và chọn `best.pt`, rồi dùng lại để báo cáo kết quả; do đó không phải test set độc lập. Cần giữ nguyên split để so sánh và tái lập kết quả.

## 6. Khó khăn và cách xử lý

| Giai đoạn | Vấn đề quan sát | Cách xử lý | Kết quả |
|---|---|---|---|
| CVAT | Automatic annotation hiển thị “No data” | Bổ sung Nuclio 1.16.3 và deploy YOLOv7 ONNX trên Docker local | Nuclio dashboard và function healthy; model xuất hiện trong CVAT. |
| Xuất nhãn | Cần chuẩn hóa định dạng YOLO `.txt` và tên file | Export YOLO Detection, sắp xếp cùng basename ảnh | Checker đạt 50/50. |
| Checker | Chạy script từ thư mục `tools` báo không tìm thấy ảnh | Chạy từ thư mục gốc repo | Đạt cấu trúc. |
| Evaluator | Ban đầu thiếu đối số và dùng cú pháp Linux trên Windows CMD | Chạy lệnh một dòng với đủ `--student-labels`, `--gold-labels` | Có kết quả đánh giá Gold QC. |
| Gold QC | F1 thấp (0.5812) dù Mean IoU cao (0.8573); nghi vấn lệch ID gold | Xem TP/FP/FN, thống kê theo ảnh, giữ nguyên gold và xin xác nhận mapping | Chưa đạt, chưa có rework cuối. |
| YOLO eval | Recall thấp, đặc biệt với bus và van; nhiều dấu hiệu nhầm lớp | Xem metrics per-class và 10 ảnh prediction | Đã có kết quả để phân tích, chưa đủ gate Gold QC chính thức. |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.
| Tiêu chí | Kết quả và mức độ hoàn thành |
|---|---|
| Định dạng YOLO (10 điểm) | **Đã đạt kiểm tra cấu trúc**: 50/50. |
| Đầy đủ vật thể (20 điểm) | Chưa bảo đảm: 254 FN theo lần chấm gold; cần kiểm nguyên nhân. |
| Độ bám sát box (20 điểm) | Mean IoU **0.8573 trên 399 box match**, nhưng còn box không match. |
| Phân lớp nhất quán (15 điểm) | Schema đã định nghĩa; cần xác minh class mapping và sửa các ca nhầm thực tế. |
| Mini guideline (10 điểm) | Đã điền bốn lớp, quy tắc box và ba trường hợp mơ hồ. |
| Gold QC (15 điểm) | **Chưa đạt F1**, chưa có số liệu sau rework. |
| Pipeline YOLO (10 điểm) | Có log train/val và ảnh prediction; **đã chạy trước khi gold gate đạt**, cần nêu đúng quy trình và bổ sung assets lên GitHub. |

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
- **CVAT Community (Docker/WSL):** Gán nhãn, kiểm tra và export YOLO Detection.
- **Google Colab + YOLO26n / Ultralytics:** Train 50 epoch, đánh giá 10 ảnh eval và xuất kết quả thực tế.
- **ChatGPT:** Giải thích cấu trúc nhãn, F1/IoU, lỗi CVAT/Docker/Python, hỗ trợ tổng hợp và diễn đạt báo cáo; dữ liệu, nhãn và nhận xét cuối cùng cần do học viên tự kiểm tra.
