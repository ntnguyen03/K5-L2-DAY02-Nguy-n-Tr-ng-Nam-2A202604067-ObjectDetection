# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` |Xe ô tô con chở người, có kích thước nhỏ đến trung bình|Sedan, Hatchback, SUV, CUV, phân biệt với các xe nhỏ khác như van bằng hình dáng xe và khoang chở hành khách, người|
| `truck` |Xe tải có chức năng chính là chở hàng hóa, vật tư |Xe tải thùng, Xe ben, xe tải nhỏ. Thường có cabin chở người và khu vực chở hàng riêng biệt|
| `bus` |Xe có kích thước lớn sử dụng để chở nhiều người |Xe bus công cộng, xe khách. Có thân dài, nhiều cửa sổ, ghế và toàn bộ là khoang chở hành khách |
| `van` |Xe dạng thân hộp, có khoang chở người hoặc hàng hóa tương đối lớn|Xe giao hàng, xe có phần chở hàng kín. Thường có trần cao, thân xe và thùng xe liền khối khác với xe ô tô thông thường|

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Vẽ bounding box hình chữ nhật bao quanh phần phương tiện nhìn thấy, sát các mép ngoài nhất có thể, hạn chế lấy thừa nền đường hoặc phương tiện khác
- Vật thể bị che khuất: Nếu vẫn nhận diện được loại xe thì tiếp tục gán nhãn và chỉ vẽ box bao quanh những phần nhìn thấy, không vẽ bbox vào những phần bị che khuất
- Vật thể bị cắt bởi rìa ảnh: Vẫn gán nhãn nếu phần hiển thị trong ảnh đủ để xác định loại xe. Không gán nhãn, kéo box ra ngoài frame ảnh
- Vật thể rất nhỏ/mơ hồ: Nếu không thể xác định được loại xe do khoảng cách, ảnh mờ hoặc bị che khuất thì không gán nhãn, tránh gán sai. Báo lại cho Team Lead

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_003.jpg` |Một số xe ở xa camera có kích thước nhỏ, hình ảnh mờ nên khó xác định chính xác loại phương tiện|Chỉ gán nhãn khi nhận diện được lớp xe; bỏ qua các đối tượng quá mờ, không đủ thông tin để phân loại|Theo guideline, vật thể nhỏ vẫn cần được gán nhãn nếu nhận diện được. Không tự suy đoán lớp khi thiếu thông tin|
| `drive_007.jpg` |Một số ô tô bị che khuất bởi phương tiện khác hoặc vật cản, chỉ nhìn thấy một phần thân xe|Vẫn gán nhãn car nếu xác định được loại xe, bounding box chỉ bao phần thân xe nhìn thấy|Schema yêu cầu gán nhãn xe bị che khuất nếu vẫn nhận diện được, không suy đoán phần thân xe bị che|
| `drive_009.jpg` |Một số phương tiện đỗ sát lề đường hoặc có hình dáng dễ nhầm lẫn giữa car, truck và van|Vẫn gán nhãn xe đang đỗ; phân loại theo đặc điểm và chức năng của xe. Xe bán tải thuộc car, xe tải chở hàng thuộc truck, xe van/minivan thuộc van|Schema phân loại theo loại phương tiện, không theo trạng thái di chuyển. Cần tuân thủ đúng định nghĩa bốn lớp|
