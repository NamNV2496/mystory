# thread

```java
#include <pthread.h>
int pthread_create(pthread_t *restrict tidp, const pthread_attr_t *restrict attr, void *(*start_rtn)(void *), void *restrict arg);
Returns: 0 if OK, error number on failure

```


## Kết thúc một thread
Thread call pthread_exit, return hoặc bị cancel bởi 1 thread khác.
	void pthread_exit(void *rval_ptr)
1 thread khác phải clear resource của thread đã exit
	int pthread_join(pthread_t thread, void **rval_ptr)
Block cho đến khi thread cần đợi kết thúc
Return EINVAL nếu thread kia đã kết thúc từ trước

## Mutex lock

Chỉ có một khóa duy nhất.
Trong một thời điểm chỉ 1 thread có được khóa
Minh họa
	pthread_mutex_t restrict mutex;
	int pthread_mutex_init(pthread_mutex_t *restrict mutex, const pthread_mutexattr_t *restrict attr);
	int pthread_mutex_destroy(pthread_mutex_t *mutex);
	int pthread_mutex_lock(pthread_mutex_t *mutex);
	int pthread_mutex_unlock(pthread_mutex_t *mutex);



