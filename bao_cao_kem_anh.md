# Báo Cáo Giải Trình Bội Số & Sơ Đồ Class Diagram

## 1. Báo Cáo Giải Trình Bội Số (Multiplicity)

Dựa vào yêu cầu bài toán, chúng ta tiến hành phân rã quan hệ Nhiều-Nhiều (* -- *) giữa `Student` và `Course` thành hai quan hệ Một-Nhiều (1 -- 0..*) thông qua lớp trung gian `Enrollment`.

*   **Quan hệ giữa Student và Enrollment (1 -- 0..*):** 
    Một học viên (Student) khi mới tạo tài khoản có thể chưa đăng ký khóa học nào (0), hoặc có thể đăng ký rất nhiều khóa học sau này (*). Tuy nhiên, mỗi một bản ghi đăng ký (Enrollment) phải luôn thuộc về duy nhất một học viên (1).

*   **Quan hệ giữa Course và Enrollment (1 -- 0..*):** 
    Một khóa học (Course) mới mở có thể chưa có ai đăng ký (0), hoặc có thể có rất nhiều học viên cùng đăng ký (*). Tương tự, mỗi bản ghi đăng ký (Enrollment) chỉ có thể tham chiếu đến một khóa học cụ thể duy nhất (1).

## 2. Sơ Đồ Class Diagram Hoàn Chỉnh

Lớp `Enrollment` được chèn vào giữa để bẻ gãy liên kết n-n và chứa thuộc tính `enrollDate`.

![Sơ đồ Class Diagram](https://kroki.io/mermaid/png/eNpLzkksLnbJTEwvSszlUgCCZJCAQnBJaUpqXolCNVgMBHSDS4oy89IViiEynilgmVokPc75pUXFqVi0JIMlsOhwzSvKz8nJRbPIJbEkVSEVLAViIumCuUrJUElBV1dByUBPT0sJ2RQrhdzE7NRisGKoc_CozUgs5gIAM_VIYA==)
