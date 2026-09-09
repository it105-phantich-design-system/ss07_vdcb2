# BÁO CÁO: KHỬ QUAN HỆ NHIỀU-NHIỀU BẰNG ASSOCIATION CLASS

## Phần 1 — Xác định Lớp trung gian (Association Class)

### 1. Vấn đề của thiết kế cũ

Trong thiết kế ban đầu, `Student` và `Course` được nối trực tiếp bằng một quan hệ nhiều-nhiều (`* -- *`):

```
Student *———— Đăng ký ————* Course
```

Về mặt logic quan hệ, một liên kết `*—*` không có "vị trí" để lưu thuộc tính riêng cho *từng cặp* (Student, Course). Khi ánh xạ sang cơ sở dữ liệu quan hệ, một quan hệ n-n không thể tồn tại dưới dạng khóa ngoại trực tiếp giữa hai bảng — nó bắt buộc phải sinh ra một bảng trung gian (bảng nối). Vì thiết kế UML hiện tại chưa mô hình hóa bảng trung gian đó thành một lớp, nên không có nơi nào để đặt thuộc tính `enrollDate` (ngày đăng ký cụ thể của một học viên cho một khóa học) — đây chính là lý do cơ sở dữ liệu "không thể lưu được ngày đăng ký cụ thể".

### 2. Giải pháp: Tạo Lớp trung gian `Enrollment`

Để khử quan hệ n-n, ta chèn một lớp trung gian gọi là **Association Class**, đặt tên là `Enrollment`, đứng giữa `Student` và `Course`. Lớp này đại diện cho **chính bản ghi đăng ký** — tức là sự kiện "một học viên cụ thể đăng ký một khóa học cụ thể" — và do đó là nơi tự nhiên để chứa thuộc tính `enrollDate: Date`.

Việc chèn lớp trung gian này tương đương với việc **phân rã một đường nối `* -- *` thành hai đường nối một-nhiều (1 — 0..*)**:

```
Student 1 ———— 0..* Enrollment 0..* ———— 1 Course
```

Trong đó:
- Mỗi `Enrollment` gắn với **đúng một** `Student` và **đúng một** `Course` (đầu số 1 phía Student/Course).
- Mỗi `Student` hoặc `Course` có thể có **nhiều** bản ghi `Enrollment` liên kết tới nó, kể cả **không có bản ghi nào** (đầu số `0..*` phía Enrollment).

### 3. Cấu trúc 3 lớp sau khi thiết kế lại

| Lớp | Vai trò | Thuộc tính chính |
|---|---|---|
| `Student` | Thực thể học viên | `-studentId: String` |
| `Course` | Thực thể khóa học | `-courseId: String` |
| `Enrollment` | **Association Class** — đại diện cho một lượt đăng ký cụ thể, nối `Student` và `Course` | `-enrollDate: Date` |

Nhờ có `Enrollment` đứng giữa, mỗi lượt đăng ký của một học viên vào một khóa học giờ đây là **một bản ghi (row) riêng biệt** trong cơ sở dữ liệu, và bản ghi đó có thể lưu được ngày đăng ký (`enrollDate`) mà không gây ra xung đột hay trùng lặp dữ liệu — vấn đề vốn không thể giải quyết được khi hai lớp `Student` và `Course` nối trực tiếp bằng quan hệ `*—*`.

### 4. Vì sao bội số phải bao gồm giá trị 0 (`0..*`)

Theo bẫy dữ liệu (edge case) của bài toán:
- Một học viên **mới tạo tài khoản** có thể **chưa đăng ký khóa học nào** → phía `Enrollment` nhìn từ `Student` phải cho phép giá trị 0, tức là `0..*` chứ không phải `1..*`.
- Một khóa học **mới mở** có thể **chưa có ai đăng ký** → tương tự, phía `Enrollment` nhìn từ `Course` cũng phải là `0..*`.

Ngược lại, phía `Student` và `Course` nhìn từ `Enrollment` luôn là `1` (bắt buộc, chính xác một), vì một bản ghi đăng ký không thể tồn tại mà không gắn với đúng một học viên và đúng một khóa học cụ thể.

*(Chi tiết bội số đầy đủ và sơ đồ hoàn chỉnh xem ở Phần 2 — file `enrollment_association_class.drawio`.)*
