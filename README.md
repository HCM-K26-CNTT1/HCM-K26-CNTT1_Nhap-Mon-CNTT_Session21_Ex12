# BÀI 12: ĐẠI TU HỆ THỐNG KIOSK SẠC ĐIỆN TOÀN THÀNH PHỐ

## PHẦN 1: TRA CỨU CẤP TỐC TÀI LIỆU & HOTFIX

### Câu 1. Sử dụng toán tử Google để tìm tệp DLL chính thức

Câu lệnh tìm kiếm:

```
site:kvaser.com "CanBus_Interface.dll" -crack -ads -download
```

Giải thích:

- `site:kvaser.com`: Chỉ tìm kiếm trên website chính thức của Kvaser.
- `"CanBus_Interface.dll"`: Tìm chính xác tên tệp DLL.
- `-crack`: Loại bỏ kết quả liên quan đến phần mềm crack.
- `-ads`: Loại bỏ kết quả quảng cáo.
- `-download`: Loại bỏ kết quả chứa từ download.

Lưu ý: Không nên tải riêng DLL từ những trang không rõ nguồn gốc. Nếu không tìm thấy tệp, hãy truy cập trang tải driver/CANlib chính thức của Kvaser và cài đặt lại đúng phiên bản tương thích với kiosk.

### Câu 2. Tìm Workaround trên GitHub Issues

Các bước thực hiện:

1. Truy cập repository GitHub của thư viện điều khiển trạm sạc.
2. Chọn tab Issues.
3. Tìm kiếm các từ khóa như `DLL missing`, `Error 126`, `driver loading`.
4. Lọc các bài viết có trạng thái OPEN để xem những lỗi đang được thảo luận.
5. Đọc phần bình luận của lập trình viên để tìm hướng xử lý tạm thời (Workaround).
6. Kiểm tra xem giải pháp có được người khác xác nhận và có phù hợp với phiên bản đang sử dụng hay không.

Lưu ý: Không áp dụng mẹo bỏ qua bước kiểm tra DLL trên hệ thống sạc thực tế. Có thể dùng bản thử nghiệm để kiểm tra, nhưng kiosk đang vận hành phải khôi phục driver đúng chuẩn hoặc rollback về phiên bản ổn định.

## PHẦN 2: ỨNG DỤNG TRỢ LÝ AI GIẢI MÃ & XỬ LÝ XUNG ĐỘT

### Câu 3. Viết Prompt 3 thành phần để giải mã lỗi EACCES

Prompt gửi ChatGPT/Gemini:

Bạn là chuyên gia kỹ thuật hỗ trợ sửa lỗi Node.js trên Windows.

**Bối cảnh:** Tôi đang quản lý hệ thống Kiosk VoltCharge. Khi ghi nhật ký hoạt động, Terminal báo lỗi:

`EACCES: permission denied, open 'C:\VoltLogs\charge.log'`

**Yêu cầu:** Hãy giải thích nguyên nhân gây ra lỗi và hướng dẫn tôi 3 bước khắc phục cụ thể, dễ hiểu. Không sử dụng lệnh nguy hiểm hoặc xóa dữ liệu.

**Kết quả mong muốn:**

1. Xác định nguyên nhân lỗi.
2. Hướng dẫn kiểm tra quyền truy cập thư mục và tệp.
3. Hướng dẫn cấp quyền ghi phù hợp cho tài khoản chạy dịch vụ và kiểm tra lại.

Giải thích 3 thành phần:

1. Vai trò: Yêu cầu AI đóng vai chuyên gia kỹ thuật.
2. Bối cảnh: Cung cấp môi trường Windows và thông báo lỗi cụ thể.
3. Yêu cầu: Đề nghị AI giải thích nguyên nhân và đưa ra 3 bước xử lý.

Cách hiểu lỗi: `EACCES` có nghĩa là chương trình không có quyền truy cập hoặc ghi vào tệp `charge.log`.

### Câu 4. Giải phóng cổng mạng 8080 bằng Terminal

Khi báo lỗi `Port 8080 already in use`, có nghĩa là cổng 8080 đang được một tiến trình khác sử dụng.

Bước 1: Tìm PID đang chiếm cổng 8080

Mở CMD hoặc Terminal và nhập:

```
netstat -ano | findstr :8080
```

- `netstat`: Kiểm tra kết nối mạng.
- `-ano`: Hiển thị cổng, địa chỉ và PID.
- `findstr :8080`: Tìm dòng chứa cổng 8080.
- PID: Mã định danh của tiến trình.

Ví dụ kết quả:

```
TCP    0.0.0.0:8080    0.0.0.0:0    LISTENING    4568
```

PID là `4568`.

Bước 2: Kiểm tra và kết thúc tiến trình

Sau khi xác định PID thuộc tiến trình không cần thiết và được phép dừng, nhập:

```
taskkill /PID 4568 /F
```

- `taskkill`: Kết thúc tiến trình.
- `/PID 4568`: Chọn tiến trình có PID 4568.
- `/F`: Buộc kết thúc tiến trình.

Bước 3: Kiểm tra lại

Chạy lại:

```
netstat -ano | findstr :8080
```

Nếu không còn tiến trình chiếm cổng thì có thể khởi động lại dịch vụ điều phối.

Lưu ý quan trọng

Không được kết thúc tiến trình chỉ vì thấy nó đang chiếm cổng 8080. Phải xác định tên và chức năng của tiến trình trước. Đặc biệt, không được tùy tiện dừng dịch vụ điều khiển sạc đang hoạt động.

## PHẦN 3: CHUẨN HÓA BUG REPORT KHẨN CẤP GỬI ĐỘI NGŨ R&D

### Câu 5. Soạn Emergency Bug Report

**1. CONTEXT – Bối cảnh**

- Hệ thống: Kiosk sạc điện VoltCharge.
- Phiên bản cập nhật: Auto-update v2.4.2.
- Phạm vi ảnh hưởng: 20 trạm sạc trên toàn thành phố.
- Thời điểm: 17:30, đúng giờ cao điểm và thời tiết mưa dông.
- Người dùng bị ảnh hưởng: Hàng trăm tài xế và người dân không thể sử dụng dịch vụ sạc bình thường.

**2. EXPECTED – Kết quả mong đợi**

- Kiosk khởi động bình thường sau khi cập nhật.
- Driver CAN Bus được tải thành công.
- Hệ thống có thể ghi nhật ký hoạt động.
- Dịch vụ điều phối khởi động được trên cổng 8080.
- Người dùng có thể sử dụng dịch vụ sạc ổn định.

**3. ACTUAL – Kết quả thực tế**

Sau khi cập nhật v2.4.2, hệ thống xuất hiện 3 lỗi nghiêm trọng:

- Lỗi 1: Không tải được driver CAN Bus, khiến dịch vụ sạc bị tê liệt.
- Lỗi 2: Không có quyền ghi tệp nhật ký charge.log.
- Lỗi 3: Cổng 8080 đã bị chiếm dụng, khiến dịch vụ điều phối không thể khởi động.

Các lỗi xuất hiện đồng loạt tại 20 trạm sạc.

**4. LOGS – Nhật ký lỗi**

Lỗi 1:\
`Failed to load driver 'CanBus_Interface.dll'. Error 126: The specified module could not be found`

Lỗi 2:\
`EACCES: permission denied, open 'C:\VoltLogs\charge.log'`

Lỗi 3:\
`Port 8080 already in use`

**5. Đề xuất xử lý khẩn cấp**

Kính đề nghị CTO và đội ngũ Backend:

1. Tạm dừng triển khai Auto-update v2.4.2 trên toàn bộ hệ thống.
2. Thực hiện Rollback về phiên bản ổn định trước đó.
3. Kiểm tra lại driver CAN Bus, quyền ghi log và cấu hình cổng 8080.
4. Kiểm thử bản vá trên môi trường thử nghiệm trước khi triển khai lại.
5. Chỉ đưa kiosk trở lại hoạt động khi đã xác nhận hệ thống sạc an toàn.

Mong đội ngũ R&D ưu tiên xử lý để giảm thiểu thiệt hại và khôi phục dịch vụ sớm nhất.

Trân trọng,

Kỹ thuật viên trưởng ca trực NOC\
VoltCharge

## KẾT LUẬN

Qua tình huống VoltCharge, em rút ra 4 kinh nghiệm:

1. Tra cứu: Biết sử dụng toán tử Google để tìm tài liệu từ nguồn chính thức.
2. Đọc lỗi: Biết tìm kiếm và tham khảo cách xử lý trên GitHub Issues, StackOverflow.
3. Sử dụng AI: Biết viết Prompt rõ ràng để AI phân tích nguyên nhân và hướng dẫn sửa lỗi.
4. Báo cáo lỗi: Biết trình bày đầy đủ Context, Expected, Actual và Logs để đội ngũ kỹ thuật xử lý nhanh chóng.

Nguyên tắc quan trọng nhất: Không sửa lỗi theo cảm tính, phải xác định nguyên nhân, kiểm tra độ an toàn của lệnh và kiểm thử trước khi đưa hệ thống trở lại hoạt động.
