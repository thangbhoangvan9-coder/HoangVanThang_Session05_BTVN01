1. Mục tiêu
Nhận biết lỗi khi sử dụng slice() để cắt chuỗi.
Hiểu cách sử dụng trim() để loại bỏ khoảng trắng đầu và cuối chuỗi.
Hiểu việc cần chuẩn hóa chuỗi trước khi kiểm tra tiền tố bằng startsWith().
Hiểu tính bất biến (Immutability) của chuỗi trong JavaScript.
Định dạng thông tin phiếu tiếp đón bệnh nhân.
2. Phân tích lỗi
Lỗi 1: Kiểm tra tiền tố trước khi chuẩn hóa chữ hoa
Mã nguồn ban đầu:
const cleanAppointmentCode = rawAppointmentCode.trim();
const isValidPrefix = cleanAppointmentCode.startsWith("MED-");
Sau khi trim(), giá trị của cleanAppointmentCode là:
med-nhi-1024
Trong khi startsWith() kiểm tra: MED-
JavaScript phân biệt chữ hoa và chữ thường nên:
"med-nhi-1024".startsWith("MED-")
cho kết quả:
false
Cách sửa
Chuẩn hóa chuỗi bằng toUpperCase() trước khi kiểm tra:
const normalizedCode = cleanAppointmentCode.toUpperCase();
const isCodeValid = normalizedCode.startsWith("MED-");
Khi đó:
MED-NHI-1024
và kết quả kiểm tra là:
true
Lỗi 2: Kiểm tra vị trí cắt số thứ tự
Với mã:
MED-NHI-1024
Các vị trí index:
M E D - N H I - 1 0 2 4
0 1 2 3 4 5 6 7 8 9 10 11
Số thứ tự 1024 bắt đầu tại index 8.

Lệnh:
slice(8, 12)
lấy các ký tự từ index 8 đến trước index 12, nên kết quả là: 1024
Do đó, với dữ liệu đề bài, slice(8, 12) thực tế không bị thiếu chữ số.
Nếu chương trình ban đầu cho rằng bị thiếu số thì cần kiểm tra lại dữ liệu đầu vào hoặc vị trí index thực tế thay vì thay đổi tùy ý tham số slice().

Test Cases
Trường hợp kiểm thử	Dữ liệu đầu vào	Kết quả sai thực tế	Kết quả đúng mong đợi
TC01 - Mã viết thường, có khoảng trắng	" med-nhi-1024 "	isValidPrefix = false do kiểm tra "MED-" trên chuỗi viết thường	Chuyên khoa NHI, số thứ tự 1024, hợp lệ true
TC02 - Mã viết hoa, có khoảng trắng	" MED-tim-2567 "	Nếu không chuẩn hóa dữ liệu có thể phát sinh sai khi kiểm tra/cắt chuỗi	Chuyên khoa TIM, số thứ tự 2567, hợp lệ true
TC03 - Mã viết thường, không có khoảng trắng	"med-noi-1234"	startsWith("MED-") trả về false nếu chưa toUpperCase()	Chuyên khoa NOI, số thứ tự 1234, hợp lệ true