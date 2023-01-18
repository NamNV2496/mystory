# signal

Là software interrupt
Là phương pháp giao tiếp phổ biến nhất giữa các app
Không thể lường trước được lúc nào thì nhận được signal
Có 32 giá trị khác nhau, mỗi giá trị được gán 1 tên riêng biệt

## Các trường hợp xảy ra signal

- User sử dụng kill command
- Process gửi signal
- Chương trình gặp lỗi
- User ấn 1 số tổ hợp phím đặc biệt

## Phân loại:
- Signal có thể ignore
- Signal không thể ignore
- Signal có thể chủ động điểu khiển được
- Signal không thể chủ động điều khiển được

## Xử lý khi nhận signal
signal.h
sighandler_t signal(int signum, sighandler_t handler)
void sig_handler(int signo)


## Một số signal cơ bản
    - SIGCHLD
    - SIGILL
    - SIGINT
    - SIGKILL   int kill(pid_t pid, int signo)
    - SIGSEGV
    - SIG_BLOCK
    - SIG_UNBLOCK
    - SIG_SETMASK


```text
#include <stdio.h>
#include <signal.h>

void sig_handler(int sig)
{
	if( sig == 9)
	{
		puts("hello world");
	}
}
int main()
{
	signal(9 , sig_handler);

	while(1);
	return 0;
}

```
```text
void handler_17(int n)
{
	printf("%d :someone is writting \n" , getpid());
}
void handler_chld(int n)
{
	printf("%d:someone write completly\n" , getpid());
	ready_to_write = 1 ; 
}
int main()
{
	// child write before parent write 
	// child signal over SIGCHLD when done to Parent 

	int pid;
	signal(17 , handler_17);
	signal(SIGCHLD , handler_chld);

	if (( pid = fork())  <0 )
	{
		perror("fork");
		return -1;
	}
	else if( pid == 0 )
	{
		//child
		printf("child : %d\n" , (int) getpid());
		kill( getppid() , 17);
		write_to_file();
	}
```


```text
		pthread_mutex_lock(&lock_valid_file);
		if(files_info[i] == 0)
		{
			files_info[i] = 1;
			ok = 1;
		}
		pthread_mutex_unlock(&lock_valid_file);
		// ghi vao file neu ok o tren 
		if( ok == 1)
		{
			
			char nameFile[30];
			sprintf(nameFile ,"fsemaphore%d.txt" , i);
			FILE *fp  = fopen(nameFile , "a+");

			fprintf( fp , "\n %lu ok write to file %d at %ld \n\n", pthread_self(), i, clock());
			sleep(2);
			files_info[i] = 0;
			fclose(fp);
			break;
		}
```
