# Process trong linux

## Process identifier

A unique number
Used to identify process
Some numbers is reserved for special process
- Swapd process
- Init process

## Tạo 1 process bằng lệnh folk

    pid_t fork(void)
Tạo ra 1 bản sao cho bộ nhớ hiện tại của process
Copy on write
    - PID thật sẽ được trả cho process con
    - PID 0 sẽ trả về cho process cha
Con có thể chạy và hoàn thành công việc trước cha

## Tạo 1 process bằng lệnh folk
    int execl(const char *pathname, const char *arg0, ... /* (char *)0 */ )
Thay the toan bo khong gian bo nhớ của process cha thanh process con
Có thể khởi tạo 1 chương trình mới
Sau khi gọi hàm exec, toàn bộ source code phía sau của chương trình sẽ không được thực hiện nữa

## Exit process
Cho dù process chủ động hoặc bị động kết thúc, 1 giá trị trạng thái kết thúc luôn được trả về cho process cha.
Sau khi gọi hàm exit và gửi trạng thái về cho process cha, process con chờ đợi tín hiệu xác nhận từ cha.
Trong khi chờ đợi tín hiệu xác nhận từ cha, process con sẽ rơi vào trạng thái zombie. Trong trạng thái này nó ko chiếm CPU, và hầu như ko tốn bộ nhớ. Tuy nhiên ko thể kill nó được.
Nếu cha kết thúc trước con thì init process sẽ làm cha mới.

## wait function
    pid_t wait(int *wstatus)
Khi 1 process con kết thúc, ngoài exit status, kernel còn gửi SIGCHILD signal cho process cha
Block process cho đến khi 1 trong các process con kết thúc




