# Chia sẻ đôi chút về lập trình web sử dụng Java Spring boot framework

## - Tại sao lại chọn lập trình web Backend (BE) mà không phải Frontend (FE)?
Như các báo đài Youtube và các kênh tiktok này kia thì đến 90% mọi người sẽ nghĩ đến lập trình web là làm với PHP, ReactJS, HTML, CSS ... Nó chính là ngôn ngữ lập trình giao diện (FE). Còn BE là phần xử lý lưu trữ data, security và các logic để có dữ liệu cho FE hiển thị ra.
-   Ví dụ page này được Nam lấy template viết bằng HTML và CSS để customize lại và nó chỉ có giao diện (FE) mà không có phần logic bên dưới (BE). Ví các bài viết được Nam load động từ file markdown ra mà không cần call API nên không được coi là lưu trữ và có BE.
## - Tại sao lại được PR và quảng báo rộng thế?
-   1 là vì nó dễ tiếp cận, dễ dạy ít tư duy. 
-   2 là các anh đó bán khóa học về chúng. Đồng thời "múa" với người ngoài ngành sẽ dễ tưởng tượng hơn BE.

## - Thế BE làm gì? Khác FE thế nào?

Phần logic (BE) sẽ làm việc xử lý các dữ liệu đầu vào của người dùng mà giao diện (FE) ghi nhận được và trả ra thông tin cần thiết. Có thể ví dụ khác biệt là BE sẽ phải quan tâm các tổ chức dữ liệu (Database) và logic còn FE thì không.
-   **Ví dụ:** Khi bạn tìm kiếm tên ai đó trên Facebook. Thì phần giao diện (FE) bạn sẽ thấy hiển thì danh sách tên những người giống vs bạn tìm kiếm. Còn việc của BE là truy vấn dữ liệu đang tồn tại để hiện thị cho bạn. Logic cách hiển thị là tìm kiếm danh sách bạn bè của bạn trước và hiển thị lên đầu, không phải bạn bè thì hiển thị sau.


## - Vậy BE sẽ làm điều lớn lao gì?

Ví dụ bây giờ bạn chuyển tiền cho 1 người khác thì việc bạn thao tác với giao diện (FE) của app: nhập ngân hàng người nhận, nhập stk người nhận rồi nhập số tiền. Thế làm cách nào mà ngân hàng đích nhận được tiền và công vào tài khoản người nhận, người chuyển bị trừ tiền và 2 bên khớp nhau để ko bị sai lệch.

=> đó chính là công việc của BE. BE sẽ làm:

- kiểm tra thời hạn đăng nhập
- Sẽ xử lý lấy lên danh sách các ngân hàng để bạn chọn
- Lấy lên thông tin như tên của người nhận dựa trên stk bạn nhập
- kiểm tra số dư vào validate số tiền bạn cần chuyển có khả dụng ko?
- kiểm tra mã pin, mật khẩu, vân tay có hợp lệ không?
- gửi lên core T24 để xử lý giao dịch
- xử lý compare 2 báo cáo cộng và trừ tiền từ 2 bên
- lưu lịch sử chuyển tiền
- notification cho app, hoặc SMS nếu đăng ký
- hỗ trợ làm sao kê

Qua đó bạn thấy rằng FE chỉ làm 1 việc còn BE làm 8-10 việc chưa. Đó là sự khó khăn của BE đó.

