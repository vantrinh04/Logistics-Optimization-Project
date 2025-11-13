# Logistics-Optimization-Project
Phân tích Chi phí Vận tải Chuỗi cung ứng Y tế (SCMS)
1. Tóm tắt
Báo cáo này phân tích chi tiết chi phí vận tải (Freight Cost) của hơn 10.000 lô hàng y tế (thuốc ARV và bộ xét nghiệm HRDT) để đánh giá hiệu quả chi tiêu của chuỗi cung ứng.
- Vấn đề: Chi phí vận tải là một khoản chi đáng kể, nhưng chúng ta chưa có cái nhìn rõ ràng về việc chi phí này chiếm bao nhiêu % giá trị hàng hóa (một KPI logistics quan trọng) và phương thức vận chuyển nào đang tốn kém nhất.
- Phát hiện chính:
1. Chi phí Air quá cao: Vận tải Hàng không (Air) có tỷ lệ chi phí/giá trị hàng trung bình (đã lọc ngoại lệ) cao gấp 5-8 lần so với Đường biển (Ocean) và Đường bộ (Truck).
2. Tồn tại "Ngoại lệ" (Outliers): Dữ liệu tồn tại nhiều lô hàng khẩn cấp có chi phí vận tải cao gấp 20 lần (2000%) giá trị hàng, cho thấy các hoạt động phản ứng (reactive) tốn kém.
3. Mạng lưới không đồng đều: Top 10 quốc gia nhận hàng nhiều nhất có sự khác biệt lớn về chi phí; một số quốc gia đang phụ thuộc quá nhiều vào vận tải Air đắt đỏ.
- Đề xuất: Thực hiện rà soát với các quốc gia có chi phí vận tải trung bình cao nhất (Top 10) để cải thiện quy trình lập kế hoạch và dự báo (Forecasting). Mục tiêu là dịch chuyển 10% các lô hàng Air (không khẩn cấp) sang Ocean trong 6 tháng tới để tiết kiệm chi phí.
2. Mục tiêu & Phương pháp
  
2.1. Mục tiêu
"Bóc tách" cơ cấu chi phí vận tải (Freight Cost) để trả lời 3 câu hỏi:
1.	Chi phí vận tải đang chiếm trung bình bao nhiêu % giá trị hàng hóa?
2.	Phương thức vận chuyển (Air, Ocean, Truck) nào hiệu quả nhất về chi phí?
3.	Quốc gia nào đang là gánh nặng chi phí vận tải lớn nhất trong mạng lưới?

2.2. Phương pháp luận Chi tiết (Excel)

Dự án được thực hiện qua 4 giai đoạn xử lý và phân tích dữ liệu:

Giai đoạn 1: Tiền xử lý & Làm sạch Dữ liệu (Data Cleansing)
- Hai cột Freight Cost (USD) và Weight (Kilograms) ban đầu ở dạng Text (Object) do chứa ký tự lạ (ví dụ: "See DN...", "Weight Captured Separately").
- Sử dụng hàm =IFERROR(VALUE(…)) để chuyển đổi 2 cột này sang dạng Số (Number). Các giá trị lỗi (text) được gán bằng 0.

Giai đoạn 2: Tạo Biến số Phân tích (Feature Engineering) Hai chỉ số KPI Logistics quan trọng đã được tạo ra cho mỗi lô hàng:
1.	Freight_Cost_Percent (% Chi phí vận tải / Giá trị hàng):
o	Mục đích: Đo lường chi phí vận tải như một tỷ lệ % của giá trị hàng hóa mà nó vận chuyển. (Đã dùng hàm IF để tránh lỗi chia cho 0).
2.	Cost_Per_Kg (Chi phí vận tải trên mỗi Kg):
o	Mục đích: Đo lường chi phí vận hành chuẩn hóa cho mỗi kg hàng.

Giai đoạn 3: Xử lý Ngoại lệ (Outlier Handling)
- Phân tích ban đầu cho thấy các giá trị Freight_Cost_Percent rất nhiễu, lên tới hơn 2000% (do các lô hàng khẩn cấp giá trị thấp).
- Để có cái nhìn thực tế về hoạt động thông thường, một bộ lọc Value Filters < 200% (hoặc 2.0) đã được áp dụng trong PivotTable, loại bỏ các trường hợp cực đoan khỏi phép tính Trung bình (Average).

Giai đoạn 4: Phân tích Tổng hợp (PivotTable Analysis) Ba phân tích PivotTable trọng tâm đã được thực hiện:
1.	PT1: Average of Freight_Cost_Percent theo Shipment Mode.
2.	PT2: Average of Cost_Per_Kg theo Shipment Mode.
3.	PT3: Average of Freight_Cost_Percent theo Country (lọc Top 10 nước nhận hàng nhiều nhất).

-> Phân tích chi tiết & Phát hiện
- Phát hiện 1: Hàng không (Air) là gánh nặng chi phí lớn nhất

Khi so sánh tỷ lệ chi phí vận tải trên giá trị hàng, sự khác biệt là rất rõ ràng.

•	Air và Air Charter (Thuê máy bay riêng) có tỷ lệ chi phí trung bình rất cao (ví dụ: 18% - 25% giá trị hàng).

•	Ocean (Đường biển) và Truck (Đường bộ) cực kỳ hiệu quả, với chi phí chỉ chiếm 2% - 5% giá trị hàng.

•	Kết luận: Mỗi đô la hàng hóa vận chuyển bằng đường hàng không tốn kém gấp 5-8 lần so với đường biển.
<img width="751" height="452" alt="image" src="https://github.com/user-attachments/assets/76802f2c-500f-4197-9f02-767410c065e1" />

- Phát hiện 2: Chi phí trên mỗi Kg khẳng định điều đó

Phân tích Cost_Per_Kg cho thấy chi phí vận hành thực tế của các phương thức:

•	Air Charter là đắt đỏ nhất (ví dụ: trung bình $15/kg).

•	Air theo sau (ví dụ: $8/kg).

•	Truck và Ocean có chi phí trên mỗi kg rất thấp (ví dụ: < $1.50/kg).

•	Kết luận: Dữ liệu này khẳng định rằng vận tải hàng không là một phương thức "xa xỉ", chỉ nên được sử dụng cho các trường hợp khẩn cấp, ưu tiên tốc độ hơn chi phí.
<img width="751" height="448" alt="image" src="https://github.com/user-attachments/assets/799f49ae-e099-4006-9420-4995e6255c02" />

- Phát hiện 3: Top 10 Quốc gia phụ thuộc vào vận tải đắt đỏ

Phân tích 10 quốc gia nhận hàng nhiều nhất (về số lượng đơn hàng) cho thấy sự chênh lệch lớn về chi phí:
<img width="737" height="516" alt="image" src="https://github.com/user-attachments/assets/3375d752-b391-40e3-9c39-536f5d188338" />

•	Các quốc gia như Zambia, Zimbabwe, Haiti (thường là các nước không có cảng biển hoặc gặp khủng hoảng) có tỷ lệ Freight_Cost_Percent trung bình cao nhất. Nguyên nhân là do các quốc gia này phụ thuộc rất lớn vào Air và Air Charter để nhận hàng khẩn cấp.

•	Ngược lại, các quốc gia như Vietnam, South Africa có chi phí tối ưu hơn nhờ tận dụng được đường biển Ocean.

4. Đề xuất & Hành động

Dữ liệu cho thấy rõ sự đánh đổi (trade-off) kinh điển của chuỗi cung ứng nhân đạo: Tốc độ (Speed) vs. Chi phí (Cost). Phải vận chuyển Air để cứu người, nhưng điều đó rất tốn kém.

Dựa trên các phát hiện, đề xuất các hành động sau:

- Chấp nhận chi phí khẩn cấp: Chấp nhận rằng một tỷ lệ chi phí Air cao (kể cả các lô vượt quá 2000%) là bắt buộc trong ngành này. Đây là chi phí "phản ứng" (Reactive Cost) để cứu bệnh nhân.

- Tối ưu hóa chi phí kế hoạch: Tập trung vào các lô hàng Air không khẩn cấp. Phòng Logistics phối hợp với Top 10 quốc gia (từ PT3) để rà soát quy trình lập kế hoạch và dự báo của họ nhằm cải thiện độ chính xác của dự báo để các quốc gia này có thể đặt hàng sớm hơn.
- Thiết lập KPI mới: Đặt mục tiêu dịch chuyển (Modal Shift) 10% các lô hàng Air (không khẩn cấp) hiện tại sang Ocean hoặc Truck trong 6 tháng tới. Điều này sẽ giúp tiết kiệm chi phí vận tải đáng kể mà vẫn đảm bảo an toàn tồn kho cho bệnh nhân.

Lưu ý: Dữ liệu thô (raw data) gốc từ Kaggle được đính kèm trong file Excel tại sheet Data Raw. Sheet này đã được ẩn (hidden) để tập trung vào phần phân tích và dashboard.

Để xem, vui lòng mở file, chuột phải vào một sheet bất kỳ và chọn "Unhide...".
