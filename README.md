# VeloPay

VeloPay là dự án ngân hàng số mô phỏng dùng để học Spring Boot qua nghiệp vụ thực tế. Khách hàng có thể theo dõi một hồ sơ từ khi đăng ký đến khi được mở tài khoản, sau đó sử dụng tài khoản để chuyển tiền, xem sao kê và yêu cầu tra soát.

Dự án đang ở giai đoạn phân tích. Các chức năng dưới đây mô tả phạm vi dự kiến. Tiền trong hệ thống là tiền giả lập, dùng VND, không thu phí và không cho thấu chi.

## Các tác nhân

| Tác nhân | Vai trò |
|---|---|
| Khách hàng | Nộp và bổ sung hồ sơ, chuyển tiền, xem số dư và sao kê, gửi yêu cầu tra soát. |
| Nhân viên nghiệp vụ | Kiểm tra hồ sơ, yêu cầu bổ sung, lập đề xuất mở tài khoản và điều tra các giao dịch cần tra soát. |
| Kiểm soát viên | Duyệt hoặc từ chối đề xuất mở tài khoản và đề xuất hoàn tiền. |
| Quản trị viên hệ thống | Quản lý tài khoản nhân viên và phân quyền truy cập. Quyền duyệt nghiệp vụ được cấp riêng. |
| Hệ thống thanh toán bên ngoài | Đối tác giả lập tiếp nhận lệnh chuyển liên ngân hàng và cung cấp kết quả để VeloPay đối chiếu. |

## Phạm vi triển khai

### 1. Mở tài khoản thanh toán

Khách hàng nộp hồ sơ. Nhân viên nghiệp vụ kiểm tra và yêu cầu bổ sung nếu thiếu thông tin. Khi hồ sơ đầy đủ, nhân viên chuyển đề xuất cho kiểm soát viên xét duyệt. Nếu được duyệt, khách hàng nhận tài khoản thanh toán. Nếu bị từ chối, khách hàng xem được lý do.

### 2. Chuyển tiền nội bộ và xem sao kê

Khách hàng đăng nhập, chọn tài khoản nguồn, nhập tài khoản nhận và số tiền. Trước khi xác nhận, khách hàng xem lại tên người nhận. Khi lệnh thành công, hai bên thấy khoản tiền ra hoặc vào trên sao kê với cùng một mã giao dịch.

- Khách hàng chỉ được chi tiền và xem sao kê trên tài khoản của mình.
- Hai tài khoản phải khác nhau, đang hoạt động và số tiền phải là số nguyên dương tính bằng đồng.
- Hệ thống kiểm tra số dư khi thực hiện lệnh. Hai lệnh đồng thời không được làm tài khoản chi vượt số dư.
- Khoản trừ ở nguồn và khoản cộng ở đích phải được ghi nhận cùng nhau. Gửi lại cùng một lệnh chỉ tạo một lần chuyển tiền.

### 3. Chuyển liên ngân hàng và tra soát

Khi đối tác chưa trả kết quả, giao dịch ở trạng thái *đang xử lý*. Khách hàng có thể theo dõi bằng mã giao dịch và gửi yêu cầu tra soát từ giao dịch gốc. Nhân viên nghiệp vụ đối chiếu kết quả với đối tác. Nếu cần hoàn tiền, nhân viên lập đề xuất để kiểm soát viên duyệt.

Người lập đề xuất không được tự duyệt. Khoản hoàn phải liên kết với giao dịch gốc và chỉ được ghi nhận một lần. Lịch sử xử lý lưu người thực hiện, thời điểm và quyết định để có thể đối chiếu sau này.

## Công nghệ dự kiến

VeloPay bắt đầu bằng một ứng dụng backend Spring Boot, chia module theo nghiệp vụ. Ba đợt triển khai ở trên cũng là thứ tự học và xây dựng dự án.

| Thành phần | Công nghệ |
|---|---|
| Backend | Java 21, Spring Boot, Maven |
| Xác thực và phân quyền | Spring Security, JWT |
| Dữ liệu và migration | PostgreSQL, Spring Data JPA, Hibernate, Flyway |
| Web và mobile | React với TypeScript, Flutter |
| Kiểm thử | JUnit, Mockito, Testcontainers |
| Tài liệu API | OpenAPI, Swagger UI |
| Đóng gói và CI/CD | Docker, GitHub Actions |
| Cache khi mở rộng | Redis |
