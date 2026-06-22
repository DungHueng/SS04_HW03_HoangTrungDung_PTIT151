# BÀI 3: Đọc hiểu & Dò lỗi qua Prompt

## Phân tích lý do prompt thô dễ bỏ sót lỗi
Prompt "Mã này bị lỗi gì?" là một prompt quá chung chung nên AI có thể chỉ tập trung vào việc kiểm tra các lỗi dễ nhận thấy như:

Lỗi cú pháp (Syntax Error). Lỗi biên dịch (Compile Error). Cảnh báo từ IDE. Quy tắc đặt tên hoặc định dạng mã nguồn.

Trong khi đó, đoạn mã trên không hề có lỗi cú pháp và vẫn biên dịch, chạy bình thường

Vấn đề nằm ở lỗi logic (Logic Error), cụ thể là vòng lặp bên trong khởi tạo: for (int j = i; j < arr.length; j++)
Khi j = i, chương trình sẽ so sánh: arr[i] == arr[i]
kiện này luôn đúng, vì một phần tử luôn bằng chính nó. Do đó, ngay lần lặp đầu tiên (i = 0, j = 0), chương trình lập tức: return arr[0];
Kết quả là hàm luôn trả về phần tử đầu tiên của mảng, kể cả khi mảng không có phần tử trùng lặp.
Ví dụ: int[] arr = {1, 2, 3, 4};
Kết quả mong muốn: null
Kết quả thực tế: 1
Nếu prompt không yêu cầu AI phân tích bằng ca kiểm thử cụ thể hoặc kiểm tra tính đúng đắn của thuật toán, AI rất dễ bỏ sót lỗi logic này vì chương trình vẫn chạy bình thường và không phát sinh lỗi trong quá trình biên dịch.

## Prompt tối ưu mới
### Hãy đóng vai trò là một Code Auditor chuyên rà soát lỗi logic trong mã nguồn Java.

### Nhiệm vụ của bạn là phân tích đoạn mã dưới đây để tìm lỗi logic, không chỉ kiểm tra lỗi cú pháp hay lỗi biên dịch.

#### Hãy sử dụng ca kiểm thử sau để chứng minh lỗi:

int[] arr = {1, 2, 3, 4};

Mảng này không có phần tử trùng lặp nên kết quả mong muốn là null, nhưng chương trình hiện tại lại trả về 1.

### Yêu cầu:

Giải thích nguyên nhân gây ra lỗi logic.

Phân tích vì sao lỗi xảy ra với ca kiểm thử trên.

Tái thiết kế thuật toán sử dụng HashSet để giảm độ phức tạp từ O(N²) xuống O(N).

Giữ nguyên kiểu dữ liệu đầu vào và đầu ra của phương thức.

Trả về toàn bộ mã nguồn Java hoàn chỉnh trong khối code markdown.

Mã nguồn Java đã sửa (sử dụng HashSet)

import java.util.HashSet; import java.util.Set;

public class DuplicateFinder {

public static Integer findDuplicate(int[] arr) {

    Set<Integer> seen = new HashSet<>();

    for (int num : arr) {
        if (seen.contains(num)) {
            return num;
        }
        seen.add(num);
    }

    return null;
}

public static void main(String[] args) {

    int[] arr1 = {1, 2, 3, 4};
    System.out.println(findDuplicate(arr1)); // null

    int[] arr2 = {5, 8, 2, 7, 8, 9};
    System.out.println(findDuplicate(arr2)); // 8

    int[] arr3 = {3, 1, 6, 2, 3};
    System.out.println(findDuplicate(arr3)); // 3
}
}
