_thread 模块提供创建新线程的方法，并提供互斥锁。
### 获取当前线程号
>_thread.get_ident()

获取当前线程号。
### 获取系统剩余内存大小
>_thread.get_heap_size()

获取系统剩余内存大小。
### 设置创建新线程使用的栈大小
>_thread.stack_size(size)

设置创建新线程使用的栈大小（以字节为单位），默认为8448字节，最小8192字节。
### 创建一个新线程
>_thread.start_new_thread(function, args)

创建一个新线程，接收执行函数和被执行函数参数，当 function 函数无参时传入空的元组。返回线程的id。
### 根据线程id删除一个线程
>_thread.stop_thread(thread_id)

删除一个线程，thread_id为创建线程时返回的线程id。当 thread_id为0时则删除当前线程。不可删除主线程。

### 创建一个互斥锁对象
>_thread.allocate_lock()

创建一个互斥锁对象。

示例：

	import _thread
	lock = _thread.allocate_lock()

### 获取锁
>lock.acquire()

获取锁，成功返回True，否则返回False。
### 释放锁
>lock.release()

释放锁。
### 返回锁的状态
>lock.locked()

返回锁的状态，True表示被某个线程获取，False则表示没有。
### 删除锁
>_thread.delete_lock(lock)

删除已经创建的锁。
### 使用示例

	import _thread
	import log
	import utime
	
	a = 0
	state = 1
	# 创建一个lock的实例
	lock = _thread.allocate_lock()
	
	def th_func(delay, id):
		global a
		global state
		while True:
			lock.acquire()  # 获取锁
			if a >= 10:
				log.info('thread %d exit' % id)
				lock.release()  # 释放锁
				state = 0
				break
			a += 1
			log.info('[thread %d] a is %d' % (id, a))
			lock.release()  # 释放锁
			utime.sleep(1)
	        
	if __name__ == '__main__':
		for i in range(2):
			_thread.start_new_thread(th_func, (i + 1, i))   # 创建一个线程，当函数无参时传入空的元组
	
		while state:
			pass
