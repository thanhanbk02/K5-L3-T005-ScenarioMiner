# PROJECT BREIF

# ScenarioMiner - Real-world Driving Scenario Mining & OpenSCENARIO Generation (bản nháp tuần 1)

Trạng thái: nháp để chốt cuối tuần 1. Mục đánh dấu **[giả định]** hoặc **[chưa kiểm]** cần xác nhận khi chạy dữ liệu thật.

## 1. Project Overview - Tổng quan dự án

**Project Name:** ScenarioMiner – Khai thác kịch bản từ log lái thực và chuyển thành kịch bản OpenSCENARIO tham số hóa.

**Product Type:** AI-powered Web Application / Autonomous Driving Testing Tool.

**Short Description:**

ScenarioMiner là một nền tảng hỗ trợ tự động khai thác các tình huống giao thông từ dữ liệu lái xe thực tế và chuyển đổi chúng thành các kịch bản kiểm thử có thể tái sử dụng trong môi trường mô phỏng xe tự lái.

Hệ thống kết hợp các phương pháp phát hiện tình huống bằng rule-base và mô hình ML/DL, trích xuất các tham số giao thông, sinh các biến thể kịch bản và xuất chúng sang định dạng OpenSCENARIO. Các kịch bản được quản lý trong một thư viện tập trung, cho phép kỹ sư tìm kiếm, kiểm tra, phê duyệt và sử dụng trong quá trình kiểm thử xe tự lái.

**Project Owner:** Phạm Thái An

**Project Status:** Project Planning / Requirement Analysis – Giai đoạn phân tích yêu cầu, xác định phạm vi, lựa chọn dataset và thiết kế giải pháp.

## 2. Background & Context – Bối cảnh dự án

### 2.1. Bối cảnh lĩnh vực

Trong lĩnh vực phát triển xe tự lái (Autonomous Driving), kiểm thử trong môi trường mô phỏng là một bước quan trọng để đánh giá khả năng xử lý của hệ thống trước nhiều tình huống giao thông khác nhau.

Một hệ thống xe tự lái cần được kiểm thử với nhiều tình huống như:

- Xe khác bất ngờ chuyển làn và cắt ngang (Cut-in).
- Phương tiện phía trước phanh gấp (Hard Braking).
- Phương tiện di chuyển ngược chiều (Wrong-way Driving).
- Người đi bộ băng qua đường (Pedestrian Crossing).

Để đánh giá khả năng xử lý của xe tự lái, đội ngũ kỹ sư cần xây dựng một bộ kịch bản kiểm thử đủ đa dạng, bao gồm cả những tình huống thông thường và các tình huống nguy hiểm, khó xử lý.

### 2.2. Quy trình hiện tại

Trong quy trình kiểm thử truyền thống, kỹ sư thường phải:

1. Xác định tình huống giao thông cần kiểm thử.
2. Tự xây dựng các actor tham gia giao thông và trạng thái ban đầu.
3. Thiết lập vị trí, vận tốc, hướng di chuyển và hành vi của từng actor.
4. Xây dựng kịch bản trong môi trường mô phỏng.
5. Điều chỉnh các tham số để tạo ra những trường hợp kiểm thử khác nhau.
6. Chạy mô phỏng và đánh giá kết quả.

Bên cạnh đó, dữ liệu lái xe thực tế từ các chuyến đi và hệ thống thu thập dữ liệu giao thông chứa nhiều tình huống có giá trị. Các dữ liệu này có thể bao gồm vị trí, vận tốc, gia tốc, hướng di chuyển và quỹ đạo của các phương tiện theo thời gian.

Tuy nhiên, dữ liệu thực tế thường tồn tại dưới dạng log thô và chưa được chuẩn hóa thành các kịch bản mô phỏng có thể tái sử dụng trực tiếp.

### 2.3. Khó khăn và hạn chế

Quy trình hiện tại tồn tại một số hạn chế:

- **Tốn thời gian xây dựng kịch bản:** Kỹ sư phải tự xác định các actor, tham số và hành vi giao thông để tạo từng tình huống kiểm thử.
- **Khó khai thác dữ liệu thực tế:** Log lái xe chứa nhiều thông tin nhưng chưa có công cụ chuyên biệt để tự động phát hiện và trích xuất các tình huống đáng chú ý.
- **Khả năng tái sử dụng hạn chế:** Dữ liệu log và kịch bản mô phỏng chưa được chuyển đổi thành một cấu trúc thống nhất, dễ chỉnh sửa và mở rộng.
- **Khó đa dạng hóa kịch bản:** Việc tạo ra nhiều biến thể của cùng một tình huống thường đòi hỏi kỹ sư thay đổi tham số thủ công.
- **Thiếu cơ chế quản lý tập trung:** Các kịch bản và kết quả kiểm thử có thể bị phân tán, khó tìm kiếm, phân loại và đánh giá.

**`Việc xây dựng một pipeline tự động khai thác log thực tế và chuyển đổi thành kịch bản mô phỏng giúp tận dụng nguồn dữ liệu giao thông sẵn có, giảm công sức xây dựng kịch bản thủ công và mở rộng khả năng kiểm thử các tình huống đa dạng cho hệ thống xe tự lái.`**

## 3. Problem Statement - Phát biểu vấn đề

Các kỹ sư phát triển và kiểm thử xe tự lái cần một lượng lớn kịch bản giao thông đa dạng để đánh giá khả năng nhận diện, dự đoán và xử lý tình huống của hệ thống.

Tuy nhiên, quá trình xây dựng những kịch bản này hiện vẫn đòi hỏi nhiều thao tác thủ công, trong khi dữ liệu lái xe thực tế chứa một lượng lớn tình huống giao thông có giá trị nhưng khó khai thác trực tiếp.

Cụ thể, các kỹ sư đang gặp khó khăn trong việc:

1. Tự động phát hiện các tình huống giao thông quan trọng từ dữ liệu log.
2. Xác định chính xác khoảng thời gian và các phương tiện liên quan đến từng tình huống.
3. Chuyển đổi dữ liệu quỹ đạo và trạng thái phương tiện thành kịch bản logic có thể tham số hóa.
4. Tạo ra nhiều biến thể của cùng một tình huống nhằm tăng độ đa dạng của bộ kiểm thử.
5. Chuyển đổi các kịch bản thành định dạng OpenSCENARIO tương thích với môi trường mô phỏng.
6. Kiểm tra tính hợp lệ, quản lý và phê duyệt các kịch bản trước khi sử dụng.

**Nguyên nhân chính:**

- Thiếu pipeline tự động kết nối giữa dữ liệu lái xe thực tế và kịch bản kiểm thử mô phỏng.
- Việc phát hiện tình huống và trích xuất tham số chưa được tự động hóa.
- Chưa có cơ chế sinh biến thể và kiểm tra các ràng buộc hình học, động học một cách có hệ thống.
- Thiếu hệ thống quản lý và kiểm duyệt tập trung đối với các kịch bản được tạo ra.

**Hậu quả nếu không giải quyết:**

- Chi phí thời gian và công sức xây dựng kịch bản kiểm thử cao.
- Khó tận dụng dữ liệu lái xe thực tế để mở rộng bộ kiểm thử.
- Bộ kịch bản có thể thiếu đa dạng và bỏ sót những tình huống nguy hiểm.
- Việc quản lý, tái sử dụng và đánh giá chất lượng kịch bản thiếu tính hệ thống.

## 4. Project Vision - Tầm nhìn sản phẩm

Xây dựng ScenarioMiner trở thành một nền tảng khai thác và quản lý kịch bản kiểm thử xe tự lái dựa trên dữ liệu giao thông thực tế.

Sản phẩm hướng tới việc kết nối dữ liệu lái xe thực tế với môi trường kiểm thử mô phỏng, giúp chuyển đổi những tình huống giao thông đã ghi nhận thành các kịch bản có cấu trúc, có khả năng tham số hóa, mở rộng và tái sử dụng.

Trong dài hạn, ScenarioMiner hướng tới một hệ sinh thái hỗ trợ kỹ sư tự động xây dựng, quản lý, đánh giá và khai thác các tình huống kiểm thử khó, góp phần nâng cao độ bao phủ và chất lượng kiểm thử hệ thống xe tự lái.

## 5. Goals & Objectives - Mục tiêu dự án

### 5.1. Business Goals

- Giảm thời gian và công sức cần thiết để xây dựng kịch bản kiểm thử giao thông.
- Tận dụng hiệu quả các dataset lái xe thực tế thay vì chỉ phụ thuộc vào những kịch bản được tạo thủ công.
- Tăng số lượng và độ đa dạng của bộ kịch bản kiểm thử.
- Tạo nền tảng quản lý tập trung, hỗ trợ tái sử dụng và chia sẻ kịch bản giữa các kỹ sư.
- Hỗ trợ phát hiện những tình huống khó và nguy hiểm có thể ảnh hưởng đến độ an toàn của hệ thống xe tự lái.

### 5.2. Product Goals

- Cho phép người dùng tải lên và xử lý dataset lái xe thực tế.
- Tự động phát hiện ít nhất 3 loại tình huống giao thông.
- Trích xuất khoảng thời gian, các actor và tham số quan trọng của từng tình huống.
- Chuẩn hóa tình huống thành Logical Scenario có thể thay đổi tham số.
- Tự động sinh các Concrete Scenario bằng các quy tắc biến đổi và Parameter Sweep.
- Xuất kịch bản sang định dạng OpenSCENARIO (.xosc).
- Hỗ trợ kiểm tra và chạy thử kịch bản trong môi trường mô phỏng được lựa chọn.
- Cung cấp thư viện quản lý, tìm kiếm và phân loại kịch bản.
- Hỗ trợ quy trình Human-in-the-loop để kỹ sư kiểm tra và phê duyệt kịch bản.

### 5.3. Technical Goals

- Xây dựng pipeline xử lý dữ liệu log có cấu trúc và chuẩn hóa thông tin của các actor.
- Kết hợp Rule-based và mô hình Machine Learning/Deep Learning để phát hiện tình huống giao thông.
- Xây dựng cơ chế trích xuất và biểu diễn tham số kịch bản.
- Triển khai bộ sinh biến thể có kiểm soát bằng Rule-based Parameter Sweep.
- Xây dựng tầng kiểm tra ràng buộc hình học và động học để loại bỏ các kịch bản không hợp lệ.
- Phát triển XML Generator tạo file OpenSCENARIO theo chuẩn đã lựa chọn.
- Đảm bảo khả năng tái hiện các tình huống từ log gốc trong môi trường mô phỏng.
- Đánh giá độ chính xác của mô hình và độ tin cậy của các kịch bản được sinh ra.

## 6. Target Users - Đối tượng người dùng

### 6.1. Primary Users - Người dùng chính

**Scenario Engineer / Autonomous Driving Test Engineer**

Là những kỹ sư phụ trách xây dựng và quản lý các kịch bản kiểm thử cho hệ thống xe tự lái.

Nhu cầu:

- Tìm kiếm và khai thác các tình huống giao thông từ dataset.
- Kiểm tra các tình huống được hệ thống phát hiện.
- Điều chỉnh tham số và sinh các biến thể kịch bản.
- Xuất kịch bản sang OpenSCENARIO.
- Chạy thử và đánh giá kịch bản trong simulator.

### 6.2. Secondary Users - Người dùng phụ

**Scenario Reviewer / QA Engineer**

Là người chịu trách nhiệm kiểm tra chất lượng và phê duyệt các tình huống được trích xuất, tham số và kịch bản trước khi đưa vào bộ kiểm thử chính thức.

Nhu cầu:

- Xem thông tin và kết quả phát hiện tình huống.
- Kiểm tra tính hợp lý của các tham số.
- Phê duyệt hoặc từ chối kịch bản.
- Theo dõi trạng thái và lịch sử kiểm duyệt.

**Ai / Data Engineer**

Là người phụ trách phát triển và duy trì pipeline xử lý dữ liệu, mô hình phát hiện tình huống và các thuật toán sinh kịch bản.

Nhu cầu:

- Kiểm tra kết quả inference.
- Đánh giá Precision, Recall và các chỉ số mô hình.
- Theo dõi dữ liệu, lỗi xử lý và chất lượng kịch bản.

### 6.3. Stakeholders - Các bên liên quan

- Nhóm phát triển ScenarioMiner.
- Giảng viên hoặc mentor phụ trách hướng dẫn dự án.
- Đội ngũ phát triển và kiểm thử hệ thống xe tự lái.
- Các đơn vị cung cấp dataset và môi trường mô phỏng.

## 7. Proposed Solution - Giải pháp đề xuất

### 7.1. Ý tưởng giải pháp

Xây dựng một nền tảng Web Application kết hợp AI và các thuật toán xác định nhằm tự động hóa quy trình chuyển đổi dữ liệu giao thông thực tế thành các kịch bản kiểm thử xe tự lái có thể chạy trong môi trường mô phỏng.

Hệ thống khai thác các thông tin quỹ đạo và trạng thái của phương tiện từ dataset, sử dụng mô hình phát hiện tình huống để nhận diện các sự kiện giao thông, sau đó chuẩn hóa chúng thành các kịch bản logic có tham số.

Từ những kịch bản logic này, hệ thống sử dụng Rule-based Parameter Sweep để tạo ra nhiều kịch bản cụ thể với các cấu hình tham số khác nhau. Các kịch bản được kiểm tra ràng buộc, chuyển đổi sang OpenSCENARIO và đưa vào thư viện để kỹ sư kiểm tra, phê duyệt và tái sử dụng.

### 7.2. High-level Workflow – Quy trình hoạt động tổng quan

**Step 1 – Data Ingestion & Preprocessing**

- Người dùng tải lên dataset lái xe thực tế.
- Hệ thống đọc và chuẩn hóa dữ liệu về các actor, thời gian, tọa độ, vận tốc, gia tốc và quỹ đạo.
- Xử lý dữ liệu thiếu, dữ liệu không hợp lệ và đồng bộ thông tin theo thời gian.

**Step 2 – Situation Detection & Extraction**

- Kết hợp Rule-based và mô hình Machine Learning/Deep Learning để nhận diện các tình huống giao thông.
- Các loại tình huống mục tiêu bao gồm Cut-in, Hard Braking, Wrong-way Driving và Pedestrian Crossing.
- Xác định khoảng thời gian xảy ra tình huống và các actor liên quan.
- Ước lượng mức độ tin cậy của kết quả phát hiện.

**Step 3 – Scenario Parameterization**

- Trích xuất các tham số quan trọng từ tình huống thực tế như tốc độ, khoảng cách, gia tốc, thời điểm và thời gian thực hiện hành động.
- Chuẩn hóa tình huống thành Logical Scenario.
- Xác định miền giá trị có thể thay đổi của các tham số.

**Step 4 – Scenario Generation & Validation**

- Sử dụng Rule-based và Parameter Sweep để sinh các Concrete Scenario từ Logical Scenario.
- Thay đổi có kiểm soát các tham số nhằm tạo ra những biến thể tình huống.
- Kiểm tra ràng buộc hình học và động học.
- Loại bỏ các kịch bản không hợp lệ hoặc trùng lặp.

**Step 5 – OpenSCENARIO Generation**

- Chuyển đổi các Concrete Scenario thành file OpenSCENARIO (.xosc).
- Sử dụng XML Template và XML Generator để đảm bảo cấu trúc đầu ra.
- Kiểm tra khả năng đọc và thực thi bằng công cụ mô phỏng đã lựa chọn.

**Step 6 – Human-in-the-loop Review**

- Kỹ sư xem xét tình huống được phát hiện và các biến thể được tạo ra.
- Reviewer có thể phê duyệt, từ chối hoặc yêu cầu điều chỉnh kịch bản.
- Chỉ các kịch bản được phê duyệt mới được đánh dấu sẵn sàng đưa vào bộ kiểm thử chính thức.

**Step 7 – Scenario Library & Evaluation**

- Lưu trữ các kịch bản và metadata.
- Hỗ trợ tìm kiếm, lọc và phân loại.
- Theo dõi trạng thái kiểm duyệt và kết quả mô phỏng.
- Cung cấp báo cáo đánh giá khả năng phát hiện tình huống và chất lượng kịch bản.

```
So với việc xây dựng kịch bản thủ công, ScenarioMiner hướng tới:
- Khai thác tự động các tình huống từ dữ liệu lái xe thực tế.
- Tạo nhiều biến thể từ cùng một tình huống gốc.
- Kiểm soát tính hợp lệ của kịch bản bằng các ràng buộc hình học và động học.
- Tự động chuyển đổi sang OpenSCENARIO.
- Quản lý và phê duyệt kịch bản trên một nền tảng tập trung.
- Hỗ trợ mở rộng bộ kiểm thử bằng dữ liệu và quy tắc sinh kịch bản mới.
```

## 8. Key Features – Các tính năng chính

### 8.1. Driving Log Management

- Tải lên dataset lái xe thực tế.
- Tiền xử lý và chuẩn hóa dữ liệu.
- Quản lý các tập log và thông tin actor.

**8.2. AI-based Situation Detection**

- Phát hiện các tình huống giao thông từ log.
- Hỗ trợ tối thiểu 3 loại tình huống.
- Trích xuất khoảng thời gian và actor liên quan.
- Hiển thị kết quả phát hiện và độ tin cậy.

**8.3. Scenario Parameterization**

- Trích xuất các tham số giao thông.
- Xây dựng Logical Scenario.
- Xác định khoảng giá trị cho từng tham số.

**8.4. Scenario Variant Generation**

- Sinh Concrete Scenario từ Logical Scenario.
- Tạo biến thể bằng Rule-based Parameter Sweep.
- Kiểm tra và loại bỏ các tổ hợp tham số không hợp lệ.

**8.5. OpenSCENARIO Generator**

- Sinh file .xosc từ Concrete Scenario.
- Hỗ trợ cấu trúc OpenSCENARIO theo phiên bản đã lựa chọn.
- Kiểm tra cú pháp và khả năng thực thi của kịch bản.

**8.6. Simulation & Evaluation**

- Chạy kịch bản trong môi trường mô phỏng được hỗ trợ.
- Kiểm tra khả năng tái hiện tình huống gốc.
- Theo dõi kết quả mô phỏng và các trường hợp va chạm.

**8.7. Scenario Library & Search**

- Lưu trữ và quản lý các kịch bản.
- Tìm kiếm theo loại tình huống, tham số và trạng thái.
- Xem chi tiết kịch bản và tải file .xosc.

**8.8. Human-in-the-loop Review**

- Hỗ trợ hai vai trò chính: Scenario Engineer và Scenario Reviewer.
- Phê duyệt hoặc từ chối kịch bản.
- Theo dõi trạng thái và lịch sử kiểm duyệt.

**8.9. Evaluation Dashboard**

- Thống kê số lượng tình huống phát hiện.
- Báo cáo Precision và Recall.
- Thống kê tỷ lệ kịch bản hợp lệ và chạy thành công.
- Theo dõi độ đa dạng của bộ kịch bản.

## 9. Scope & Out of Scope – Phạm vi dự án

### 9.1. In Scope – Phạm vi thực hiện

Trong phiên bản đầu tiên (MVP), nhóm tập trung vào:

**Data Processing**

- Sử dụng một hoặc một số dataset công khai có dữ liệu trajectory phù hợp.
- Ưu tiên dữ liệu dạng có cấu trúc như JSON, CSV hoặc định dạng bảng tương đương.
- Chuẩn hóa dữ liệu về một schema nội bộ thống nhất.

**Situation Detection**

- Phát hiện tối thiểu 3 loại tình huống giao thông.
- Kết hợp Rule-based và mô hình ML/DL phù hợp với dữ liệu.
- Đánh giá kết quả bằng nhãn tham chiếu được gán thủ công.

**Scenario Parameterization & Generation**

- Trích xuất các tham số quan trọng.
- Xây dựng Logical Scenario.
- Sinh các Concrete Scenario bằng các quy tắc biến đổi tham số.
- Có cơ chế kiểm tra ràng buộc hình học và động học.

**OpenSCENARIO & Simulation**

- Sinh file .xosc theo một phiên bản OpenSCENARIO XML được lựa chọn.
- Ưu tiên tích hợp với một môi trường mô phỏng để kiểm chứng kịch bản.
- Đánh giá khả năng tái hiện tình huống gốc.

**Web Application**

- Quản lý dataset và các tình huống được phát hiện.
- Quản lý thư viện kịch bản.
- Hỗ trợ tìm kiếm, lọc và xem chi tiết.
- Triển khai quy trình Scenario Engineer – Scenario Reviewer.
- Cung cấp dashboard đánh giá cơ bản.

**Data Safety**

- Chỉ sử dụng dữ liệu công khai hoặc dữ liệu đã được ghi nhận và cho phép sử dụng.
- Ẩn danh các thông tin nhận dạng cá nhân nếu có.
- Kịch bản chỉ được sử dụng trong môi trường mô phỏng, không điều khiển phương tiện thực tế.

### 9.2. Out of Scope – Ngoài phạm vi

Trong phiên bản đầu tiên, nhóm chủ động không triển khai:

- Thu thập dữ liệu lái xe thực tế bằng hệ thống cảm biến hoặc phương tiện thử nghiệm riêng.
- Phân tích video camera giao thông từ đầu đến cuối bằng một pipeline Computer Vision/VLM hoàn chỉnh, trừ khi dataset được lựa chọn yêu cầu.
- Huấn luyện một mô hình foundation model hoặc VLM từ đầu.
- Xây dựng hệ thống điều khiển phương tiện tự lái thực tế.
- Triển khai kiểm thử trên xe thật hoặc môi trường giao thông thực tế.
- Hỗ trợ mọi loại dataset, bản đồ và định dạng kịch bản khác nhau.
- Hỗ trợ đồng thời tất cả các simulator và toàn bộ tính năng của OpenSCENARIO.
- Xây dựng một hệ thống Root Cause Analysis hoàn chỉnh có khả năng xác định chính xác module gây lỗi bên trong hệ thống xe tự lái.
- Xây dựng hệ thống Automatic Hard Case Mining và tối ưu hóa tham số nâng cao ở quy mô công nghiệp.

Các chức năng nâng cao có thể được xem xét trong những phiên bản tiếp theo nếu thời gian và nguồn lực cho phép.

## 10. Success Metrics – Tiêu chí đánh giá thành công

Các chỉ số sau là mục tiêu đề xuất cho MVP và sẽ được hiệu chỉnh sau khi nhóm khảo sát dataset, xây dựng baseline và thống nhất trong PRD.

### 10.1. Situation Detection Performance

- Phát hiện được ít nhất 3 loại tình huống giao thông.
- Có tập dữ liệu kiểm thử với nhãn tham chiếu được gán thủ công.
- Đánh giá Precision, Recall và F1-score cho từng loại tình huống.
- Mục tiêu ban đầu: Macro Precision và Macro Recall đạt từ 0.80 trở lên trên tập kiểm thử được gán nhãn.

### 10.2. Scenario Generation

- Có khả năng tạo nhiều Concrete Scenario từ một Logical Scenario.
- Hỗ trợ ít nhất 3 quy tắc sinh biến thể cho một loại tình huống được lựa chọn.
- Các biến thể phải có cấu hình tham số khác nhau và không trùng lặp hoàn toàn.
- Có thể thống kê chính xác số kịch bản ứng viên được tạo ra trước và sau quá trình validation.

### 10.3. Scenario Validation & OpenSCENARIO

- Có cơ chế kiểm tra các ràng buộc hình học và động học.
- File .xosc tuân thủ cấu trúc của phiên bản chuẩn đã lựa chọn.
- Mục tiêu ban đầu: Ít nhất 90% kịch bản được phê duyệt có thể được parser của simulator mục tiêu đọc và thực thi thành công.
- Lưu lại trạng thái và thông tin lỗi của các kịch bản không hợp lệ.

### 10.4. Scenario Reconstruction

- Đánh giá mức độ tương đồng giữa quỹ đạo trong log gốc và quỹ đạo tái hiện trong simulator.
- Sử dụng các chỉ số như Position MAE/RMSE và Velocity MAE.
- Xác định sai số chấp nhận được sau khi kiểm tra hệ tọa độ, bản đồ và phương pháp ánh xạ dữ liệu sang simulator.

### 10.5. Scenario Diversity

- Có khả năng tạo ra nhiều biến thể từ các tình huống gốc.
- Các biến thể thể hiện sự khác biệt về tốc độ, khoảng cách, thời gian và các tham số liên quan.
- Có thể thống kê số lượng kịch bản duy nhất theo từng loại tình huống và từng miền tham số.

### 10.6. Human-in-the-loop

- Có ít nhất hai vai trò: Scenario Engineer và Scenario Reviewer.
- Các kịch bản được đưa vào quy trình phê duyệt trước khi đánh dấu là sẵn sàng cho bộ kiểm thử chính thức.
- Có khả năng theo dõi trạng thái Pending, Approved và Rejected.

## 11. Constraints & Assumptions – Giới hạn và giả định

### 11.1. Constraints – Giới hạn

**Time Constraint**

- Tổng thời gian phát triển: 5 tuần.
- Ưu tiên hoàn thiện một pipeline MVP chạy được từ đầu vào đến đầu ra thay vì phát triển quá nhiều chức năng nâng cao.

**Human Resource Constraint**

- Nhóm có số lượng 4 thành viên.
- Cần phân chia công việc rõ ràng giữa Data/AI, Backend, Frontend và Integration/Testing.

**Budget Constraint**

- Ưu tiên dataset công khai, thư viện mã nguồn mở và các công cụ miễn phí.
- Hạn chế phụ thuộc vào API hoặc dịch vụ thương mại có chi phí sử dụng.

**Hardware Constraint**

- Sử dụng tài nguyên máy tính và GPU hiện có của nhóm.
- Ưu tiên mô hình và thuật toán có chi phí tính toán phù hợp.
- Việc sử dụng simulator cần được kiểm tra khả năng đáp ứng phần cứng trước khi lựa chọn.

**Technology Constraint**

- Đầu ra chính là OpenSCENARIO XML (.xosc).
- Phải lựa chọn phiên bản OpenSCENARIO và simulator tương thích.
- Kịch bản chỉ được chạy trong môi trường mô phỏng.
- Pipeline cần có khả năng kiểm tra tính hợp lệ của dữ liệu và kịch bản.

### 11.2. Assumptions – Giả định

**Dataset Assumptions**

- Dataset đầu vào có thể truy cập hợp pháp và được phép sử dụng.
- Dữ liệu chứa các thông tin cần thiết như actor ID, timestamp, position, velocity và trajectory.
- Đối với các tình huống cần xác định làn đường hoặc hướng giao thông, dataset có thông tin bản đồ/lane hoặc có thể bổ sung thông tin tương ứng.
- Đối với Pedestrian Crossing, dataset có thông tin về actor người đi bộ và quỹ đạo di chuyển của họ.
- Nhóm có thể gán nhãn thủ công một tập dữ liệu nhỏ để đánh giá mô hình.

**User Assumptions**

- Người dùng có kiến thức cơ bản về giao thông và các tình huống kiểm thử xe tự lái.
- Kỹ sư có khả năng kiểm tra và đánh giá các tham số được hệ thống trích xuất.
- Reviewer chịu trách nhiệm phê duyệt kịch bản trước khi sử dụng chính thức.

**Simulation Assumptions**

- Simulator được lựa chọn hỗ trợ phiên bản OpenSCENARIO mà nhóm triển khai.
- Có bản đồ, mô hình actor và các tài nguyên cần thiết để tái hiện kịch bản.
- Có thể ánh xạ hệ tọa độ và quỹ đạo của dataset sang hệ tọa độ trong môi trường mô phỏng với sai số chấp nhận được.

**Technical Assumptions**

- Các tình huống có thể được biểu diễn bằng các tham số và quy tắc xác định.
- Rule-based và Parameter Sweep đủ đáp ứng chức năng sinh biến thể cơ bản.
- Mô hình ML/DL có thể được huấn luyện hoặc áp dụng trên dữ liệu đã được chuẩn hóa và gán nhãn.
- Các thuật toán hình học và động học có thể được sử dụng để kiểm tra tính hợp lệ của những kịch bản được sinh ra.

## 12. Timeline & Deliverables – Tiến độ và bàn giao

Tổng thời gian thực hiện: 5 tuần.

**Week 1 – Research, Dataset Selection & System Design**

**Objectives:**

- Nghiên cứu bài toán Scenario Mining và OpenSCENARIO.
- Khảo sát, lựa chọn dataset công khai phù hợp.
- Xác định các loại tình huống mục tiêu.
- Phân tích cấu trúc dữ liệu và xây dựng schema nội bộ.
- Thiết kế kiến trúc tổng thể và phân chia công việc.

**Deliverables:**

- Dataset được lựa chọn và tài liệu mô tả.
- Data schema và quy trình preprocessing.
- Danh sách loại tình huống mục tiêu.
- System Architecture và High-level Workflow.
- Thiết kế sơ bộ Logical Scenario và Concrete Scenario.

**Week 2 – Data Processing & Situation Detection**

**Objectives:**

- Xây dựng pipeline đọc và tiền xử lý dữ liệu.
- Chuẩn hóa trajectory và thông tin actor.
- Phát triển các quy tắc phát hiện tình huống ban đầu.
- Huấn luyện hoặc tích hợp mô hình ML/DL phù hợp.
- Xây dựng tập dữ liệu đánh giá và kiểm tra kết quả Detection.

**Deliverables:**

- Data Preprocessing Pipeline.
- Situation Detection Module.
- Kết quả nhận diện ít nhất 3 loại tình huống.
- Báo cáo Precision, Recall và F1-score ban đầu.

**Week 3 – Scenario Parameterization & Variant Generation**

**Objectives:**

- Xây dựng module trích xuất tham số từ tình huống.
- Chuẩn hóa các tình huống thành Logical Scenario.
- Xác định miền giá trị của các tham số.
- Phát triển Rule-based Parameter Sweep để sinh Concrete Scenarios.
- Xây dựng bộ kiểm tra ràng buộc hình học và động học.

**Deliverables:**

- Scenario Parameterization Module.
- Logical Scenario Representation.
- Rule-based Variant Generator.
- Scenario Validation Module.
- Bộ Concrete Scenarios được tạo từ dữ liệu thực tế.

**Week 4 – OpenSCENARIO Generation & Web Application**

**Objectives:**

- Xây dựng XML Generator sử dụng template.
- Xuất Concrete Scenarios sang OpenSCENARIO.
- Kiểm tra cấu trúc và khả năng thực thi trong simulator mục tiêu.
- Phát triển Web Application với các chức năng cốt lõi.
- Xây dựng Scenario Library và quy trình Human-in-the-loop.

**Deliverables:**

- OpenSCENARIO XML Generator.
- Các file .xosc có thể kiểm tra/chạy trên simulator mục tiêu.
- Web Application MVP.
- Scenario Library và chức năng tìm kiếm.
- Cơ chế Scenario Engineer – Scenario Reviewer.

**Week 5 – Integration, Evaluation & Final Demonstration**

**Objectives:**

- Tích hợp toàn bộ pipeline từ Data Ingestion đến Scenario Library.
- Kiểm thử quy trình end-to-end.
- Đánh giá chất lượng phát hiện tình huống.
- Đánh giá tỷ lệ kịch bản OpenSCENARIO hợp lệ và chạy thành công.
- Đánh giá mức độ tái hiện log gốc và độ đa dạng kịch bản.
- Sửa lỗi, hoàn thiện tài liệu và chuẩn bị demo.

**Deliverables:**

- ScenarioMiner MVP hoàn chỉnh.
- Pipeline xử lý và sinh kịch bản end-to-end.
- Scenario Library có thể tìm kiếm và quản lý.
- Bộ kịch bản OpenSCENARIO được sinh ra từ dataset.
- Báo cáo đánh giá và kết quả thực nghiệm.
- Tài liệu kiến trúc, hướng dẫn sử dụng và tài liệu kỹ thuật.
- Slide và kịch bản trình diễn sản phẩm.

```
**Final Expected Outcome**

Sau 5 tuần, nhóm hướng tới bàn giao một phiên bản MVP của ScenarioMiner có khả năng tiếp nhận dataset lái xe thực tế, tự động phát hiện ít nhất 3 loại tình huống giao thông, trích xuất và tham số hóa các tình huống, sinh các biến thể bằng Rule-based, kiểm tra tính hợp lệ, xuất thành file OpenSCENARIO có thể chạy trong simulator mục tiêu và quản lý các kịch bản thông qua một Web Application có hỗ trợ Human-in-the-loop.

Sản phẩm chứng minh được khả năng tự động hóa một phần đáng kể quy trình chuyển đổi từ dữ liệu lái xe thực tế sang thư viện kịch bản kiểm thử cho xe tự lái.
```
