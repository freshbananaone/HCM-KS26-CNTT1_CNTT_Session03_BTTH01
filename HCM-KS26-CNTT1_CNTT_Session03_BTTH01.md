# Bài thực hành 1: Trích xuất và định dạng thông tin tuyển dụng

## 1. Prompt sử dụng

```text
[C - Context]
Bạn là chuyên viên xử lý dữ liệu nhân sự cấp cao của một công ty công nghệ.

[R - Request]
Hãy đọc một email ứng tuyển và lấy ra 6 thông tin:
- Họ tên (name)
- Số điện thoại (phone)
- Email (email)
- Vị trí ứng tuyển (position)
- Số năm kinh nghiệm (experience_years)
- Kỹ năng chính (skills)

[E - Explanation]
Thông tin được dùng để nhập vào database tuyển dụng nên phải chính xác, đúng dữ liệu trong email. Không được đoán hoặc tự thêm thông tin.

[A - Action]
1. Đọc toàn bộ email, tìm thông tin tương ứng với 6 trường.
2. Viết hoa chữ cái đầu của từng phần trong họ tên, ví dụ: "NGUYỄN VĂN AN" thành "Nguyễn Văn An".
3. Xóa khoảng trắng và dấu phân cách thừa trong số điện thoại. Nếu là số Việt Nam bắt đầu bằng 0 và đủ điều kiện nhận diện, có thể đổi sang đầu số +84 (bỏ số 0 đầu).
4. Giữ nguyên nội dung vị trí ứng tuyển nhưng sửa cách viết cho dễ đọc.
5. Ghi kinh nghiệm bằng số năm; ví dụ "2 năm" -> 2. Không suy ra kinh nghiệm từ năm học hoặc số năm đi học.
6. Tách các kỹ năng thành danh sách, xóa kỹ năng trùng nhau, thống nhất cách viết như Python, SQL, React.
7. Nếu thiếu thông tin thì để null trong JSON và "Không có thông tin" trong bảng. Nếu không nhắc đến kỹ năng thì skills = null; chỉ dùng [] khi email nói rõ không có kỹ năng.
8. Chỉ xuất đúng 2 phần theo thứ tự: một bảng Markdown gồm 2 cột "Trường" và "Giá trị", sau đó là một mảng JSON gồm 1 đối tượng có đúng 6 khóa name, phone, email, position, experience_years, skills.
9. JSON phải hợp lệ: dùng dấu ngoặc kép cho chuỗi và tên khóa; dùng null (không đặt trong dấu nháy); experience_years là số hoặc null; skills là mảng chuỗi hoặc null. Không đặt dấu phẩy sau phần tử cuối.
10. Không viết lời chào, nhận xét, giải thích hay kết luận ở đầu và cuối kết quả. Phần JSON là dữ liệu thuần, không thêm chú thích.

[T - Tone]
Trình bày ngắn gọn, khách quan và rõ ràng.

[E - Extras: 2 ví dụ Few-shot]

Ví dụ 1 - Input:
"Chào phòng nhân sự, em là NGUYỄN VĂN AN. Em muốn ứng tuyển Backend Developer. Số điện thoại của em là 090 123 4567, email: an.nguyen@example.com. Em có 2 năm kinh nghiệm làm Python, SQL, FastAPI và Python."

Ví dụ 1 - Output:
| Trường | Giá trị |
|---|---|
| Họ tên | Bình Minh |
| Số điện thoại | +848948xxxx |
| Email | binhminh@example.com |
| Vị trí ứng tuyển | Backend Developer |
| Năm kinh nghiệm | 2 |
| Kỹ năng chính | Python, SQL, FastAPI |

[
  {
    "name": "Bình Minh",
    "phone": "+848948xxxx",
    "email": "binhminh@example.com",
    "position": "Backend Developer",
    "experience_years": 2,
    "skills": ["Python", "SQL", "FastAPI"]
  }
]

Ví dụ 2 - Input:
"Em tên là trần thị bình, email thapvo@hotmail.com. Em xin ứng tuyển vị trí thực tập sinh Frontend. Em biết HTML, CSS, JavaScript và HTML. Em cảm ơn ạ."

Ví dụ 2 - Output:
| Trường | Giá trị |
|---|---|
| Họ tên | Võ Trần Tháp |
| Số điện thoại | Không có thông tin |
| Email | thapvo@hotmail.com |
| Vị trí ứng tuyển | Thực tập sinh Frontend |
| Năm kinh nghiệm | Không có thông tin |
| Kỹ năng chính | HTML, CSS, JavaScript |

[
  {
    "name": "Võ Trần Tháp",
    "phone": +8434xxxxxx,
    "email": "thapvo@hotmail.com",
    "position": "Thực tập sinh Frontend",
    "experience_years": null,
    "skills": ["HTML", "CSS", "JavaScript"]
  }
]

[BẮT ĐẦU EMAIL CẦN XỬ LÝ]
{{EMAIL_THO}}
[KẾT THÚC EMAIL]

Hãy xử lý email trên theo đúng quy tắc và định dạng của 2 ví dụ.
```

## 2. Kiểm tra JSON

Em đã kiểm tra hai mảng JSON trong ví dụ bằng Python `json.loads()`. Cả hai đều hợp lệ, có đủ 6 trường và có thể dùng để nhập dữ liệu sau khi tách phần JSON khỏi bảng Markdown.