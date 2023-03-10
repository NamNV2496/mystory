# Exception

### Exception ?

Là 1 sự kiện xảy ra trong quá trình thực thi một chương trình java, Exception phá vỡ flow (luồng xử lý) bình thường của một chương trình, làm gián đoạn chương trình của chúng ta

### Error ?

Là những vấn đề nghiêm trọng liên quan đến môi trường thực thi của ứng dụng hoặc hệ thống mà lập trình viên không thể kiểm soát. Nó thường làm chết chương trình  

### Phân biệt Exception and Error

- Exception: Có thể xử lý được
- Error: Không thể xử lý

### Ví dụ

- Exception: FileNotFoundException, NullPointerException, ArrayIndexOutOfBoundException ...
- Error: Out of Memory Error, Virtual Machine Errror

![img.png](blog/java/img/exception.png)

## EXCEPTION TYPES

![img.png](blog/java/img/exception1.png)


## EXCEPTION HANDLING

Exception handling trong java là một cơ chế để xử lý các lỗi runtime để duy trì luồng bình thường của ứng dụng.
Quá trình xử lý exception được gọi là catch exception, nếu Runtime System không xử lý được ngoại lệ thì chương trình sẽ kết thúc


![img.png](blog/java/img/exception2.png)

![img.png](blog/java/img/exception3.png)

## PROCESSING SYNTAX

### 1.1 Try – catch block
Khối lệnh try được sử dụng để chứa 1 đoạn code có thể xảy ra ngoại lệ
Đi theo sau try thường là catch,  Khối catch được sử dụng để xử lý các exception.

![img.png](blog/java/img/exception4.png)

### 1.2 Multi-block catch

Khi  thực hiện nhiều tác vụ và với mỗi tác vụ có thể xảy ra các exception khác nhau thì nên sử dụng multi catch để xử lý hết các exception đó.

![img.png](blog/java/img/exception5.png)

### 1.3 Nested Try
Khi một nhỏ của khối lệnh có thể xảy ra 1 exception khác, và toàn bộ khối lệnh cũng có thể xảy ra một exception khác. Ta nên sử dụng nested try

![img.png](blog/java/img/exception6.png)

### 1.4  Finally (Block)
Finally được sử dụng để thực thi code quan trọng, nó luôn được thực thi cho dù ngoại lệ được xử lý hay không. 

![img.png](blog/java/img/exception7.png)

### 2.1 Throw 
Sử dụng để ném ra một ngoại lệ rõ ràng
- Sau throw là một instance
- Throw có thể dùng ở mọi nơi (Sau dùng try –catch để bắt lỗi đó hoặc dung throws để ném cho 1 bên khác xử lý.
- Throw chỉ ném ra được 1 ngoại lệ


![img.png](blog/java/img/exception8.png)

### 2.2  Throws
Sử dụng để thông báo có exception và phải ném exception đó vào 1 hàm try-catch để xử lý.
Sau throws thường là 1 class
Throws được khai báo ở đầu phương thức
Có thể throws nhiều exception


![img.png](blog/java/img/exception9.png)







