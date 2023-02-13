# Java OOP

## OOP (Object Oriented Programming) là gì? Tại sao cần quan tâm?

OOP Là một phương pháp hay mô hình giúp tăng năng suất, đơn giản hóa việc bảo trì, dễ dàng mở rộng trong thiết kế phần mềm bởi việc cung cấp một vài khái niệm như:
- Lớp (Class)
- Đối tượng (Object)
- Trừu tượng (Abstraction)
- Đóng gói (Encapsulation)
- Kế thừa (Inheritance)
- Đa hình (Polymorphism)

![img.png](blog/java/img/OOP.png)


=> Tóm lại OOP sẽ là bản chất của vấn đề, là cách để ta triển khai hướng đối tượng, là khung xương để ta implement

## THẾ MẠNH CỦA OOPS SO VỚI NGÔN NGỮ LẬP TRÌNH HƯỚNG THỦ TỤC


-  Lập trình hướng đối tượng giúp việc phát triển và bảo trì dễ dàng hơn. Trong khi phương pháp lập trình hướng thủ tục là không dẽ dàng quản lý khi code lớn.
- Lập trình hướng đối tượng có tính năng ẩn dấu thông tin, trong khi hướng thủ tục có thể truy cập dữ liệu toàn cục ở bất kỳ nơi nào.
- Lập trình hướng đối tượng cung cấp khả năng mô phỏng sự kiện thực tế hiệu quả hơn.

### **LỚP (Class)**
Một lớp là một nhóm đối tượng có các thuộc tính chung. Nó là một mẫu hoặc thiết kế từ đó các đối tượng được tạo ra.

+) Một lớp trong java có thể chứa:
- Thành viên dữ liệu
- Constructor
- Phương thức
- Khối lệnh
- Lớp và Interface

### **ĐỐI TƯỢNG (Object)**

Một thực thể có trạng thái và hành vi được gọi là đối tượng. Ví dụ như máy pha cà phê, xe đạp, cái quạt...

+) Một đối tượng có ba đặc điểm:
- Trạng thái: Đại diện cho dữ liệu (giá trị) của một đối tượng.
- Hành vi: Đại diện cho hành vi (chức năng) của một đối tượng như gửi tiền, rút tiền,...
- Danh tính: Danh tính của một đối tượng thường được cài đặt thông qua một ID duy nhất. ID này được ẩn đối với user bên ngoài. Tuy nhiên nó được sử dụng trong nội bộ máy ảo JVM để định danh từng đối tượng.
**Ví dụ:** Bút chì là một đối tượng. Tên của nó là A, màu trắng, ... được gọi là trạng thái. Nó được sử dụng để viết, viết được gọi là hành vi.

+) Đối tượng(Object) là một thể hiện của một lớp(Class). Lớp là một mẫu hoặc thiết kế từ đó các đối tượng được tạo ra. Vì vậy, đối tượng là các thể hiện (kết quả) của một lớp.
- Cách tạo đối tượng trong Java: Sử dụng từ khóa new, sử dụng phương thức newInstance(), sử dụng phương thức clone(), sử dụng phương thức factory…

### **TRỪU TƯỢNG (Abstraction)**

Tính trừu tượng là một tiến trình ẩn các cài đặt chi tiết và chỉ hiển thị tính năng tới người dùng.
Nói cách khác, nó chỉ hiển thị các thứ quan trọng tới người dùng và ẩn các chi tiết nội tại, ví dụ: để gửi tin nhắn, người dùng chỉ cần soạn text và gửi tin. Bạn không biết tiến trình xử lý nội tại về phân phối tin nhắn.
Tính trừu tượng giúp bạn trọng tâm hơn vào đối tượng thay vì quan tâm đến cách nó thực hiện.

- 2 giai đoạn: Design và implementation

- Thể hiện qua abstract class và interface

#### **Abstract class**

Một lớp được khai báo với từ khóa abstract là lớp abstract trong Java. Lớp abstract có nghĩa là lớp trừu tượng, nó có thể có các phương thức abstract hoặc non-abtract.


#### **Interface**
- Một Interface trong Java là một bản thiết kế của một lớp. Nó chỉ có các phương thức trừu tượng. Interface là một kỹ thuật để thu được tính trừu tượng hoàn toàn và đa kế thừa trong Java. Interface trong Java cũng biểu diễn mối quan hệ IS-A. Nó không thể được khởi tạo giống như lớp trừu tượng.
- Nói cách khác, các trường của Interface là public, static và final theo mặc định và các phương thức là public và abstract. Các method của nó đều là method trừu tượng, nghĩa là không có thân hàm.
- Một Interface trong Java là một tập hợp các phương thức trừu tượng (abstract). Một class triển khai một interface, do đó kế thừa các phương thức abstract của interface.
- Một interface không phải là một lớp. Viết một interface giống như viết một lớp, nhưng chúng có 2 định nghĩa khác nhau. Một lớp mô tả các thuộc tính và hành vi của một đối tượng. Một interface chứa các hành vi mà một class triển khai.
- Một interface không chứa bất cứ hàm Constructor nào.
- Tất cả các phương thức của interface đều là abstract.
- Một interface không thể chứa một trường nào trừ các trường vừa static và final.
- Một interface không thể kế thừa từ lớp, nó được triển khai bởi một lớp.
- Một interface có thể kế thừa từ nhiều interface khác.

#### **So sánh Abstract Class vs Interface**

![img.png](blog/java/img/abstractVsInterface.png)


Interface: Barkable, Runable, Flyable, Swimable.
Abstract class Animal và các sub class: Bolt, AngryBird và Nemo.
Abstract class Machine và các sub class: McQueen, Siddeley.

![img.png](blog/java/img/abstract.png)
![img.png](blog/java/img/interface.png)


## **Encapsulation**

Đóng gói trong Java là một cơ chế gói dữ liệu (biến) và mã tác động lên dữ liệu (phương thức) lại với nhau như một đơn vị duy nhất (class,package…). Trong tính năng đóng gói, các biến của một lớp sẽ bị ẩn khỏi các lớp khác và chỉ có thể được truy cập thông qua các phương thức của lớp hiện tại của chúng. Do đó, nó còn được gọi là ẩn dữ liệu.

+) Để đạt được tính đóng gói trong Java: 
- Khai báo các biến của một lớp là private.
- Cung cấp các phương thức setter và getter công khai để sửa đổi và xem các giá trị của biến.

## **Inheritance**

Kế thừa trong java là sự liên quan giữa hai class với nhau, trong đó có class cha (superclass) và class con (subclass). Khi kế thừa class con được hưởng tất cả các phương thức và thuộc tính của class cha. Tuy nhiên, nó chỉ được truy cập các thành viên public và protected của class cha. Nó không được phép truy cập đến thành viên private của class cha.

- Tư tưởng của kế thừa trong java là có thể tạo ra một class mới được xây dựng trên các lớp đang tồn tại. Khi kế thừa từ một lớp đang tồn tại bạn có sử dụng lại các phương thức và thuộc tính của lớp cha, đồng thời có thể khai báo thêm các phương thức và thuộc tính khác.
- Cú pháp: sử dụng từ khóa **extends** để kế thừa 


![img.png](blog/java/img/inheritance.png)

### **Is-a**

Trong Java, mối quan hệ Is-A phụ thuộc vào tính kế thừa. Kế thừa xa hơn có hai loại, kế thừa lớp và kế thừa giao diện. Nó được sử dụng để tái sử dụng mã trong Java. Ví dụ: Khoai tây là rau, Xe buýt là phương tiện, v.v. Một trong những thuộc tính của thừa kế là tính kế thừa có tính chất một chiều. Giống như chúng ta có thể nói rằng một ngôi nhà là một công trình. Nhưng không phải tất cả các công trình đều là nhà ở. Chúng ta có thể dễ dàng xác định mối quan hệ Is-A trong Java. Khi có một từ khóa extend hoặc implement trong khai báo lớp trong Java, thì lớp cụ thể được cho là tuân theo mối quan hệ Is-A.

### **Has-a**

Trong Java, mối quan hệ Has-A còn được gọi là composition. Nó cũng được sử dụng để tái sử dụng mã trong Java. Trong Java, quan hệ Has-A chỉ đơn giản có nghĩa là một thể hiện của một lớp có tham chiếu đến một thể hiện của một lớp khác hoặc một thể hiện khác của cùng một lớp. Ví dụ, một chiếc ô tô có một động cơ, một con chó có một cái đuôi, v.v. Trong Java, không có từ khóa nào như vậy thực hiện mối quan hệ Has-A. Nhưng chúng tôi chủ yếu sử dụng các từ khóa new để triển khai mối quan hệ Has-A trong Java.

## **Polymorphism**

Đa hình trong java (Polymorphism) là một khái niệm mà chúng ta có thể thực hiện một hành động bằng nhiều cách khác nhau.
- 2 kiểu đa hình: Đa hình lúc biên dịch (compile) và đa hình lúc thực thi (runtime)
- Đa hình runtime là quá trình gọi phương thức thông qua đối tượng được tạo.
- Đa hình compile time (static polymorphism) là quá trình gọi phương thức thông qua lớp.

### **Overloading** 

- Overloading trong java xảy ra nếu một lớp có nhiều phương thức có tên giống nhau nhưng khác nhau về kiểu dữ liệu hoặc số lượng các tham số.
- Sử dụng nạp chồng phương thức giúp tăng khả năng đọc hiểu chương trình.

### **Overriding**

- Ghi đè phương thức trong java xảy ra nếu lớp con có phương thức giống lớp cha.
- Nói cách khác, nếu lớp con cung cấp sự cài đặt cụ thể cho phương thức đã được cung cấp bởi một lớp cha của nó được gọi là ghi đè phương thức (method overriding) trong java.
- Sử dụng cho đa hình runtime

+) Nguyên tắc
- Phương thức phải có tên giống với lớp cha.
- Phương thức phải có tham số giống với lớp cha.
- Lớp con và lớp cha có mối quan hệ kế thừa. 


![img.png](blog/java/img/overloading.png)



