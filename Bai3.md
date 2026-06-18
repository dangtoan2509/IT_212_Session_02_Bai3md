# Bài 3: Đọc hiểu & Dò lỗi qua Prompt (Phát hiện Ảo giác AI - Hallucination)

## 1) Tại sao prompt nêu phương thức không tồn tại dễ dẫn đến hallucination?
Khi prompt cung cấp tên phương thức giả định (ví dụ `StringUtils.hasSpecialCharacters()`), AI có xu hướng tin vào dữ kiện đầu vào và sinh mã gọi phương thức đó dù thực tế API không có. Nguyên nhân chính:
- Mô hình cố gắng tối ưu hóa phản hồi văn bản phù hợp với yêu cầu người dùng, nên sẽ *sáng tạo* (hallucinate) để trả lời một cách mạch lạc.
- Nếu prompt định hướng hay gợi mẫu sai, mô hình không có cơ chế kiểm tra thực thi/biên dịch để phản hồi đúng/sai.

Kết quả: mã nguồn bị sinh ra sẽ tham chiếu API không tồn tại, gây lỗi biên dịch.

## 2) Prompt tối ưu (yêu cầu AI kiểm chứng nguồn)
Hãy viết một phương thức Java `containsSpecialCharacter(String input)` kiểm tra chuỗi có chứa ký tự đặc biệt hay không. Yêu cầu:
- Không sử dụng bất kỳ phương thức không chính thức hay không tồn tại trong thư viện nào.
- Chỉ được dùng API tiêu chuẩn Java (`java.lang`, `java.util`, `java.util.regex`, v.v.) hoặc thư viện phổ biến đã được kiểm chứng; nếu bạn đề xuất dùng thư viện bên ngoài (ví dụ Apache Commons), hãy chỉ rõ tên thư viện, phiên bản và link tài liệu chính thức chứng minh phương thức bạn dùng tồn tại.
- Bắt buộc giải thích ngắn gọn vì sao regex được chọn (nếu dùng regex) và dẫn nguồn tới tài liệu Java chính thức hoặc trang của thư viện nếu sử dụng thư viện ngoài.

## 3) Mã nguồn Java do AI sinh ra (theo prompt tối ưu)
```java
package vn.example.util;

import java.util.regex.Pattern;

public class StringHelpers {

    // Regex: tìm bất kỳ ký tự nào không phải chữ, số hoặc khoảng trắng
    private static final Pattern SPECIAL_CHAR_PATTERN = Pattern.compile("[^a-zA-Z0-9\\s]");

    /**
     * Kiểm tra chuỗi có chứa ký tự đặc biệt hay không.
     * Trả về true nếu tìm thấy ký tự không phải chữ, số, hoặc khoảng trắng.
     */
    public static boolean containsSpecialCharacter(String input) {
        if (input == null || input.isEmpty()) return false;
        return SPECIAL_CHAR_PATTERN.matcher(input).find();
    }

    public static void main(String[] args) {
        System.out.println(containsSpecialCharacter("Hello World")); // false
        System.out.println(containsSpecialCharacter("Hello@World")); // true
    }
}
```