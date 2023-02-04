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
              