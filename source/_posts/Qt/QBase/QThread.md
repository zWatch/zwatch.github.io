需要注意包括：优先级（Priority），事件（eventDispatcher）。

# 槽：

```C++
void quit()
void start(QThread::Priority priority = InheritPriority)
void terminate()
```

# 优先级

```C++
enum QThread::Priority{	
    //Constant Value Description

    IdlePriority = 0, //懒惰的，当没有其它线程在活动时调用此线程。
    LowestPriority = 1, //最低优先级
    LowPriority = 2, //低优先级
    NormalPriority = 3, //默认的优先级
    HighPriority = 4, //比默认的优先级高一些
    HighestPriority = 5, //比高优先级还高
    TimeCriticalPriority = 6, //尽可能的调用
    InheritPriority = 7, //继承优先级
}
```

# 信号

```c++
void finished()
void started()
```

用户不能自己调用。

# 包含函数

```C++
int exec()
virtual void run()
```



# 静态公开成员

```C++
QThread * create(Function &&f, Args &&... args);
QThread * create(Function &&f);
QThread * currentThread();
Qt::HANDLE currentThreadId();
int idealThreadCount();
void msleep(unsigned long msecs);
void sleep(unsigned long secs);
void usleep(unsigned long usecs);
void yieldCurrentThread();
```



# 函数
 `void setTerminationEnabled(bool enabled = true)`

QThread对象管理程序中的一个控制线程。QThreads开始在`run（）`中执行。默认情况下，`run（）`通过调用`exec（）`启动事件循环，并在线程内运行Qt事件循环。

你可以转移工作对象（Object），通过使用`QObject::moveToThread()`

```C++
QThread::QThread(QObject *parent = nullptr)
```

Constructs a new QThread to manage a new thread. The *parent* takes ownership of the QThread. The thread does not begin executing until [start](qthread.html#start)() is called.

构造一个QThread对象来管理一个线程。Parent将获取Qthread的所有权。线程不会开始在 `start()` 被调用前。

```C++
[[signal]] 
void QThread::finished()
```



This signal is emitted from the associated thread right before it finishes executing.

这个信号将会从相关联的线程发出，当它结束之前。

When this signal is emitted, the event loop has already stopped running. No more events will be processed in the thread, except for deferred deletion events. This signal can be connected to [QObject::deleteLater](qobject.html#deleteLater)(), to free objects in that thread.

当这个信号发出时，事件循环已经停止。没有更多的事件将会被发送给进程除了延迟删除事件。这个信号可以连接到`QObject::deleteLater` 来释放这个线程下的资源

Note: If the associated thread was terminated using [terminate](qthread.html#terminate)(), it is undefined from which thread this signal is emitted.

当相关线程时使用`terminate()` 结束的，则无法确认信号是哪个线程发出的。

Note: This is a private signal. It can be used in signal connections but cannot be emitted by the user.

你不能调用这个信号。

See also [started](qthread.html#started)().

```C++
[[signal]]
void QThread::started()
```

这个信号当线程开始运行的时候被发送，在run函数被调用之前。（私有不应当自己调用）



```C++
[[slot]] 
void QThread::quit()
```

Tells the thread's event loop to exit with return code 0 (success). Equivalent to calling [QThread::exit](qthread.html#exit)(0).

通知线程的事件循环该退出了，退出返回值是0，相当于`QThread::exit(0)`。

This function does nothing if the thread does not have an event loop.

如果线程没有事件循环，这个函数不做任何事情。



```C++
[[slot]] 
void QThread::terminate()
```

Terminates the execution of the thread. The thread may or may not be terminated immediately, depending on the operating system's scheduling policies. Use [QThread::wait](qthread.html#wait)() after terminate(), to be sure. When the thread is terminated, all threads waiting for the thread to finish will be woken up.

结束线程的运行。这个线程可能（有可能不）被立即停止，者取决于操作系统的调度策略，使用 `QThread::wait` 在terminate（）之后，以确保当线程结束，所有等待该线程结束的线程被唤醒。

**警告**：这个函数相当危险，代码可能结束在任何地方，此时资源不一定会被释放，锁不一定会解除。除非必要，使用`setTerminationEnabled(bool).` 来延时线程的终止知道设置为true。



```C++
virtual QThread::~QThread()
```

Note that deleting a [QThread](qthread.html) object will not stop the execution of the thread it manages. Deleting a running [QThread](qthread.html) (i.e. [isFinished](qthread.html#isFinished)() returns false) will result in a program crash. Wait for the [finished](qthread.html#finished)() signal before deleting the [QThread](qthread.html).

删除一个线程（QThread）并不会停止线程的运行。删除一个正在运行中的QThread（当isFinished返回false）将会导致程序崩溃。等待finished信号在删除之前。



```C++
static template<typename Function, typename Args> QThread* QThread::create(Function &&f, Args &&... args)
```

The new thread is not started -- it must be started by an explicit call to [start](qthread.html#start)(). This allows you to connect to its signals, move QObjects to the thread, choose the new thread's priority and so on. The function *f* will be called in the new thread.

这个线程不会开始，直到显式调用`start()`。这允许你在此之前链接它的信号，移动Objects到线程，重新更改线程的优先级。这个函数将会在新的线程里被调用。

Note: the caller acquires ownership of the returned [QThread](qthread.html) instance.

调用者需要拥有QThread的所有权。

Note: this function is only available when using C++17.

这个只能在C++17可用。

Warning: do not call [start](qthread.html#start)() on the returned [QThread](qthread.html) instance more than once; doing so will result in undefined behavior.

不能再一个实例上调用多次start()，这将导致未定义行为。

这个函数在Qt5.10引入。





```C++
static QThread *QThread::currentThread()
```

返回指向管理当前执行线程的QThread的指针。



```C++
static Qt::HADNLE QThread::currentThreadId()
```

Returns the thread handle of the currently executing thread.

Warning: The handle returned by this function is used for internal purposes and should not be used in any application code.

Note: On Windows, this function returns the DWORD (Windows-Thread ID) returned by the Win32 function GetCurrentThreadId(), not the pseudo-HANDLE (Windows-Thread HANDLE) returned by the Win32 function GetCurrentThread().

没太看懂。



```C++
QAbstractEventDispatcher *QThread::eventDispatcher() const
```

返回指向事件分派器的指针。 事件分派器也得看一下





```C++
protected:
int QThread::exec()
```

Enters the event loop and waits until [exit](qthread.html#exit)() is called, returning the value that was passed to [exit](qthread.html#exit)(). The value returned is 0 if [exit](qthread.html#exit)() is called via [quit](qthread.html#quit)().

进入事件循环知道exit被调用，返回值就是传给exit的值。

This function is meant to be called from within [run](qthread.html#run)(). It is necessary to call this function to start event handling.

要启动事件循环，这个函数就必须在run函数里调用。

See also [quit](qthread.html#quit)() and [exit](qthread.html#exit)().



```C++
void QThread::exit(int returnCode = 0)
```

注意这里的返回和C/C++中的不同，这里停止的是事件循环而不是线程。



```C++
static int QThread::idealThreadCount()
```

返回理想状态下的可运行线程数，这个函数会询问处理器的核心数量，包括虚拟的和物理的。将会返回1如果无法检测。

```C++
bool QThread::isInterruptionRequested() const
void QThread::requestInterruption()

```

返回true如果有中止请求。

# 一些状态判断函数

`int QThread::loopLevel() const`

`bool QThread::isRunning() const`



```C++
static void QThread::sleep(unsigned long *secs*)
```

避免直接使用此函数，如果是在等待变量改变，可以将其链接到信号，或在事件处理中调用。





```C++
bool QThread::wait(QDeadlineTimer deadline = QDeadlineTimer(QDeadlineTimer::Forever))
```

QT5.15



```C++
static void QThread::yieldCurrentThread()
```

