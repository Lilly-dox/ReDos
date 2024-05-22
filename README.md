# ReDos
Regular expression Denial of Service (ReDos) là một loại tấn công khiến cho việc xử lý biểu thức chính quy (regex) yêu cầu thời gian cực lớn, gây chậm trễ hoặc treo hệ thống . Điều này dẫn đến số lượng các lần kiếm tra kết hợp phải tăng theo cấp số nhân với độ dài chuỗi đầu vào.


Quay lui thảm họa (catastrophic backtracking) xảy ra khi một biểu thức chính quy (regex) phải kiểm tra một số lượng lớn các nhánh tiềm năng trước khi quyết định rằng không có khớp. Điều này có thể dẫn đến thời gian xử lý tăng lên một cách không tỷ lệ với độ dài của đầu vào, đặc biệt là khi đầu vào không khớp với mẫu regex.
Ví dụ về Catastrophic Backtracking
Giả sử chúng ta có biểu thức chính quy sau đây:
```(a|aa)*b```
Biểu thức này cố gắng khớp với chuỗi gồm các ký tự 'a' hoặc 'aa' lặp lại nhiều lần, kết thúc bằng 'b'.
Ví dụ cụ thể
```
'b': Khớp (vì (a|aa)* có thể khớp với chuỗi trống, sau đó là 'b').
'ab': Khớp (vì (a|aa)* khớp với 'a', sau đó là 'b').
'aab': Khớp (vì (a|aa)* khớp với 'aa', sau đó là 'b').
'aaaab': Khớp (vì (a|aa)* khớp với 'aaaa', sau đó là 'b').
'aaaaaaab': Khớp (vì (a|aa)* khớp với bảy 'a' (có thể phân tích thành các tổ hợp 'a' và 'aa'), sau đó là 'b')
```
Chúng ta lấy chuỗi đầu vào: 'aaaaaaaaaaaaaaaaaaaaaaaaa'
 Chuỗi này gồm 24 ký tự 'a' liên tiếp và không có 'b' ở cuối. Vì không có 'b' ở cuối, regex engine sẽ cố gắng khớp từng phần của chuỗi với (a|aa)* theo nhiều tổ hợp khác nhau.
# Quá trình khớp (Matching Process)
Khi regex engine gặp chuỗi này, nó sẽ thử từng tổ hợp của 'a' và 'aa' để tìm cách khớp, dẫn đến quay lui nhiều lần. Đây là một số bước cơ bản mà regex engine có thể thực hiện:

- Bước đầu tiên:

Thử khớp toàn bộ chuỗi 'aaaaaaaaaaaaaaaaaaaaaaaa' với (a|aa)*.
Thử khớp các phần tử 'a' từng cái một (24 lần 'a').

- Bước tiếp theo:
Sau khi khớp 24 lần 'a', engine nhận ra rằng không có 'b' để khớp ở cuối.
Engine bắt đầu quay lui và thử các tổ hợp khác của 'a' và 'aa'.

- Backtracking:
  + Thử khớp 23 lần 'a', rồi 1 lần 'aa', vẫn không có 'b' ở cuối.
  + Thử khớp 22 lần 'a', rồi 2 lần 'aa', vẫn không có 'b' ở cuối.
 Cứ tiếp tục như vậy, engine thử từng tổ hợp có thể của 'a' và 'aa'.
Ví dụ cụ thể về tổ hợp
 + Tổ hợp đầu tiên: 24 lần 'a' và không có 'b'.
 + Tổ hợp thứ hai: 23 lần 'a', 1 lần 'aa', không có 'b'.
 + Tổ hợp thứ ba: 22 lần 'a', 2 lần 'aa', không có 'b'.
 + Tổ hợp thứ tư: 21 lần 'a', 3 lần 'aa', không có 'b'.
Và cứ tiếp tục như vậy cho đến khi engine thử tất cả các tổ hợp có thể của 'a' và 'aa'.
- Kết quả
Vì không có 'b' ở cuối, tất cả các tổ hợp đều thất bại. Quá trình này có thể rất tốn thời gian vì số lượng các tổ hợp là rất lớn, dẫn đến quay lui thảm họa.

