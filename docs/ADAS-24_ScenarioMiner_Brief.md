# ADAS-24 ScenarioMiner — Brief (bản nháp tuần 1)

Trạng thái: nháp để chốt cuối tuần 1. Mục đánh dấu **[giả định]** hoặc **[chưa kiểm]** cần xác nhận khi chạy dữ liệu thật.

## 1. Vấn đề

Kịch bản kiểm thử ADAS viết tay tốn công và dễ lệch khỏi thực tế. Log lái thực chứa rất nhiều tình huống thật (xe cắt ngang, phanh gấp, người đi bộ băng đường) nhưng log thô khó tái sử dụng: chưa có công cụ trích đúng đoạn tình huống rồi chuyển thành kịch bản chạy được, đổi được tham số.

## 2. Giải pháp (một câu)

Web app nạp log lái công khai, tự phát hiện tình huống, trích tham số thành kịch bản logic, sinh kịch bản OpenSCENARIO chạy được trong esmini, cho kỹ sư duyệt (human-in-the-loop) rồi đưa vào thư viện kịch bản có tag và tìm kiếm.

## 3. Người dùng

| Vai trò | Làm gì |
|---|---|
| Kỹ sư kịch bản | Nạp log, xem tình huống phát hiện được, chỉnh khoảng tham số, gửi duyệt, xuất `.xosc` |
| Người duyệt | Xem tình huống và tham số, so với log gốc, chấp nhận / từ chối / yêu cầu sửa |

## 4. Phạm vi

**Làm (bắt buộc, theo yêu cầu cơ bản của đề tài)**
- Nạp một tập log công khai và chuẩn hóa về một schema chung.
- Phát hiện ≥ 3 loại tình huống: cut-in, phanh gấp, người đi bộ băng ngang.
- Xuất kịch bản OpenSCENARIO chạy được trong esmini.
- Thư viện kịch bản có tag và tìm kiếm.
- Báo cáo precision / recall so với nhãn tay.
- 2 vai trò và luồng duyệt HITL.

**Làm nếu còn thời gian (nâng cao)**
- Quét tham số để sinh kịch bản cụ thể từ kịch bản logic.
- Gom nhóm bằng HDBSCAN và loại trùng để tăng độ đa dạng.
- Đo độ khớp giữa kịch bản tái hiện và log gốc (sai số quỹ đạo).

**Không làm**
- Không chạy trên xe thật; chỉ chạy mô phỏng.
- Không xin dữ liệu riêng hay dữ liệu có PII.
- Không huấn luyện mô hình dự đoán quỹ đạo.

## 5. Dữ liệu

- **Chính:** Argoverse 2 Motion Forecasting (250.000 scenario, 6 thành phố, 10 Hz). Giấy phép CC BY-NC-SA 4.0 (chỉ dùng phi thương mại).
- **Cho bản nháp:** 100–300 scenario của split val (nhanh nhất qua Kaggle, nhưng bản Kaggle do bên thứ ba đăng, license ghi "Unknown", chỉ dùng thử).
- **Tự làm:** ≥ 50 scenario gán nhãn tay để tính precision / recall.
- **[chưa kiểm]** Dữ liệu có chứa track của xe ego hay không (tài liệu không nói rõ). Kiểm tra ngay tuần 1.

## 6. Kết quả mong đợi sau 4 tuần

| Tuần | Mốc |
|---|---|
| 1 | Brief, PRD, Wireframe; loader đọc 1 scenario Argoverse 2; kiểm tra có track ego không; chạy được 1 loại sự kiện đầu-cuối |
| 2 | Đủ 3 bộ phát hiện sự kiện; bộ nhãn tay; số precision / recall đầu tiên |
| 3 | Sinh `.xosc` cho cả 3 loại, chạy esmini; backend FastAPI; thư viện kịch bản |
| 4 | Giao diện web với luồng duyệt HITL; báo cáo đánh giá; demo |

## 7. Chỉ số thành công

Đo cả 4 chỉ số theo đề tài. Ngưỡng mục tiêu **[giả định, chốt sau khi có số đo cơ sở ở tuần 2]**.

| Chỉ số | Cách đo |
|---|---|
| Precision / recall phát hiện | So với ≥ 50 scenario nhãn tay |
| Tỷ lệ xuất hợp lệ | % file `.xosc` chạy hết kịch bản trong esmini không lỗi |
| Độ khớp quỹ đạo | RMSE giữa mô phỏng và log gốc, tính tương đối theo ego |
| Độ đa dạng | Số cụm HDBSCAN và tỷ lệ trùng bị loại |

Số tham chiếu từ demo (log tổng hợp, không phải dữ liệu thật): sai số khoảng cách dọc 1.12 m, ngang 0.15 m cho một cut-in.

## 8. Ràng buộc

- GPU 2080 Ti, 4 tuần, có thể dùng Kaggle (đĩa khoảng 20 GB).
- Kỹ sư duyệt kịch bản trước khi vào bộ kiểm thử chính thức.
- Chỉ dùng log công khai, ẩn danh; chỉ chạy mô phỏng.
- Slot tối đa 2 người.

## 9. Rủi ro và cách xử lý

| Rủi ro | Xử lý |
|---|---|
| Không có track ego | Chọn một xe làm ego giả theo luật, ghi rõ trong báo cáo |
| Bản đồ Argoverse không phải OpenDRIVE | Dùng đường thẳng tự sinh, giữ quan hệ tương đối giữa các xe |
| Dữ liệu thiên lệch (chọn cảnh nhiều tương tác) | Ghi trong mục giới hạn; không nêu tần suất sự kiện như số liệu đường thật |
| Ngưỡng phát hiện sai | Hiệu chỉnh bằng nhãn tay, báo cáo precision / recall |
| Scope phình | Khóa 3 loại sự kiện; phần nâng cao chỉ làm khi mục cơ bản xong |
| Esmini không hiện xe 3D | Không ảnh hưởng chạy và đo; xem thêm gói demo / mô hình nếu cần |

## 10. Câu hỏi mở

1. Làm một mình hay hai người (slot tối đa 2)?
2. Có track ego trong dữ liệu không?
3. Dùng Kaggle hay máy 2080 Ti làm môi trường chính?
4. Có dùng thêm LLM hỗ trợ gán nhãn / mô tả tình huống (phần nâng cao) hay không?
