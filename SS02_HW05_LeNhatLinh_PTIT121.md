1. Bối cảnh và ý đồ thiết kế
Hệ thống GreenBus cần kiểm tra mã vé theo đúng quy tắc nghiệp vụ. Để giảm lỗi do AI sinh mã nguồn sai logic, áp dụng quy trình 2 bước:
Bước 1: AI sinh mã nguồn TicketValidator.
Bước 2: Một AI khác đóng vai trò kiểm toán độc lập để rà soát, phản biện và phát hiện lỗi tiềm ẩn trước khi sử dụng.
Cách làm này giúp tăng độ tin cậy và chất lượng mã nguồn.

2. Prompt sinh mã nguồn (Bước 1)
Hãy viết lớp Java TicketValidator để kiểm tra mã vé GreenBus.
Quy tắc:
Bắt đầu bằng "BUS-".
Tiếp theo là 2 ký tự chữ in hoa đại diện tỉnh/thành.
Tiếp theo là "-" và 6 chữ số ngày đi theo định dạng YYMMDD.
Ngày đi không được nhỏ hơn ngày hiện tại.
Yêu cầu:
Xử lý trường hợp null hoặc chuỗi rỗng.
Kiểm tra định dạng bằng Regex.
Kiểm tra ngày hợp lệ (không chấp nhận ngày 32 tháng 13,...).
Sử dụng thư viện java.time.LocalDate.
Trả về true nếu hợp lệ, false nếu không hợp lệ.
Viết thêm hàm main để kiểm thử.
3. Prompt kiểm chứng độc lập (Bước 2)
Bạn là Kỹ sư kiểm thử và kiểm toán mã nguồn Java.
Hãy phân tích mã nguồn TicketValidator dưới đây và tìm lỗi tiềm ẩn:
NullPointerException.
Sai định dạng Regex.
Sai khi phân tích YYMMDD.
Sai khi xử lý ngày không hợp lệ.
Sai khi chuyển năm mới.
Sai do múi giờ hệ thống.
Sai khi so sánh ngày quá khứ.
Nếu phát hiện lỗi:
Giải thích nguyên nhân.
Đưa ví dụ gây lỗi.
Đề xuất cách sửa.
Sinh lại phiên bản mã nguồn tối ưu hơn bằng LocalDate.

4. Minh chứng thực tế
Mã nguồn AI sinh lần đầu
public boolean isValid(String code) {
    return code.matches("BUS-[A-Z]{2}-\\d{6}");
}
Kết quả AI kiểm duyệt

Phát hiện lỗi:

Không kiểm tra null → dễ gây NullPointerException.
Chỉ kiểm tra Regex, không kiểm tra ngày hợp lệ.
Chấp nhận mã:
BUS-HN-261332

(tháng 13, ngày 32).

Không kiểm tra ngày trong quá khứ.
Mã nguồn cuối cùng sau kiểm duyệt
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.time.format.ResolverStyle;

public class TicketValidator {

    private static final String REGEX =
            "^BUS-[A-Z]{2}-\\d{6}$";

    public static boolean isValid(String code) {

        if (code == null || code.isBlank()) {
            return false;
        }

        if (!code.matches(REGEX)) {
            return false;
        }

        try {
            String datePart = code.substring(7);

            DateTimeFormatter formatter =
                    DateTimeFormatter.ofPattern("uuMMdd")
                            .withResolverStyle(ResolverStyle.STRICT);

            LocalDate travelDate =
                    LocalDate.parse(datePart, formatter);

            return !travelDate.isBefore(LocalDate.now());

        } catch (Exception e) {
            return false;
        }
    }
}