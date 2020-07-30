- [Q](../qtdoc/index.html)t 5.15
- [Qt Core](qtcore-index.html)
- [C++ Classes](qtcore-module.html)
- QThread
- [Q](qtcore-index.html)t 5.15.0 Reference Documentation 

### Contents

- [Public Types](#public-types)公开类型
- [Public Functions](#public-functions)公开函数
- [Reimplemented Public Functions](#reimplemented-public-functions)公开的重实现函数
- [Public Slots](#public-slots)公开的槽
- [Signals](#signals)信号
- [Static Public Members](#static-public-members)静态成员
- [Protected Functions](#protected-functions)
- [Static Protected Members](#static-protected-members)
- [Detailed Description](#details)
- [Managing Threads](#managing-threads)

# QThread Class

The QThread class provides a platform-independent way to manage threads.

QThread类提供了独立于平台方法来管理线程。 [More...](#details)



| Header:   | #include <QThread>      |
| --------- | ----------------------- |
| qmake:    | QT += core              |
| Inherits: | [QObject](qobject.html) |

- [List of all members, including inherited members](qthread-members.html) 

## Public Types



| enum | [Priority](qthread.html#Priority-enum) { IdlePriority, LowestPriority, LowPriority, NormalPriority, HighPriority, …, InheritPriority } |
| ---- | ------------------------------------------------------------ |
| 枚举 | 优先级：懒惰的，最低级，低级，正常，高级，...，继承。        |

## Public Functions



|                            | [QThread](qthread.html#QThread)(QObject **parent* = nullptr) |
| -------------------------- | ------------------------------------------------------------ |
| virtual                    | [~QThread](qthread.html#dtor.QThread)()                      |
| QAbstractEventDispatcher * | [eventDispatcher](qthread.html#eventDispatcher)() const      |
| void                       | [exit](qthread.html#exit)(int *returnCode* = 0)              |
| bool                       | [isFinished](qthread.html#isFinished)() const                |
| bool                       | [isInterruptionRequested](qthread.html#isInterruptionRequested)() const |
| bool                       | [isRunning](qthread.html#isRunning)() const                  |
| int                        | [loopLevel](qthread.html#loopLevel)() const                  |
| QThread::Priority          | [priority](qthread.html#priority)() const                    |
| void                       | [requestInterruption](qthread.html#requestInterruption)()    |
| void                       | [setEventDispatcher](qthread.html#setEventDispatcher)(QAbstractEventDispatcher **eventDispatcher*) |
| void                       | [setPriority](qthread.html#setPriority)(QThread::Priority *priority*) |
| void                       | [setStackSize](qthread.html#setStackSize)(uint *stackSize*)  |
| uint                       | [stackSize](qthread.html#stackSize)() const                  |
| bool                       | [wait](qthread.html#wait)(QDeadlineTimer *deadline* = QDeadlineTimer(QDeadlineTimer::Forever)) |
| bool                       | [wait](qthread.html#wait-1)(unsigned long *time*)            |

## Reimplemented Public Functions



| virtual bool | [event](qthread.html#event)(QEvent **event*) override |
| ------------ | ----------------------------------------------------- |
|              |                                                       |

## Public Slots



| void | [quit](qthread.html#quit)()                                  |
| ---- | ------------------------------------------------------------ |
| void | [start](qthread.html#start)(QThread::Priority *priority* = InheritPriority) |
| void | [terminate](qthread.html#terminate)()                        |

## Signals



| void | [finished](qthread.html#finished)() |
| ---- | ----------------------------------- |
| void | [started](qthread.html#started)()   |

## Static Public Members



| QThread *  | [create](qthread.html#create)(Function &&*f*, Args &&... *args*) |
| ---------- | ------------------------------------------------------------ |
| QThread *  | [create](qthread.html#create-1)(Function &&*f*)              |
| QThread *  | [currentThread](qthread.html#currentThread)()                |
| Qt::HANDLE | [currentThreadId](qthread.html#currentThreadId)()            |
| int        | [idealThreadCount](qthread.html#idealThreadCount)()          |
| void       | [msleep](qthread.html#msleep)(unsigned long *msecs*)         |
| void       | [sleep](qthread.html#sleep)(unsigned long *secs*)            |
| void       | [usleep](qthread.html#usleep)(unsigned long *usecs*)         |
| void       | [yieldCurrentThread](qthread.html#yieldCurrentThread)()      |

## Protected Functions



| 签名         |                             |
| ------------ | --------------------------- |
| int          | [exec](qthread.html#exec)() |
| virtual void | [run](qthread.html#run)()   |

## Static Protected Members



| void | [setTerminationEnabled](qthread.html#setTerminationEnabled)(bool *enabled* = true) |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

## Detailed Description

A QThread object manages one thread of control within the program. QThreads begin executing in [run](qthread.html#run)(). By default, [run](qthread.html#run)() starts the event loop by calling [exec](qthread.html#exec)() and runs a Qt event loop inside the thread.

You can use worker objects by moving them to the thread using [QObject::moveToThread](qobject.html#moveToThread)().

```
 class Worker : public QObject
 {
     Q_OBJECT

 public slots:
     void doWork(const QString &parameter) {
         QString result;
         /* ... here is the expensive or blocking operation ... */
         emit resultReady(result);
     }

 signals:
     void resultReady(const QString &result);
 };

 class Controller : public QObject
 {
     Q_OBJECT
     QThread workerThread;
 public:
     Controller() {
         Worker *worker = new Worker;
         worker->moveToThread(&workerThread);
         connect(&workerThread, &QThread::finished, worker, &QObject::deleteLater);
         connect(this, &Controller::operate, worker, &Worker::doWork);
         connect(worker, &Worker::resultReady, this, &Controller::handleResults);
         workerThread.start();
     }
     ~Controller() {
         workerThread.quit();
         workerThread.wait();
     }
 public slots:
     void handleResults(const QString &);
 signals:
     void operate(const QString &);
 };
```

The code inside the Worker's slot would then execute in a separate thread. However, you are free to connect the Worker's slots to any signal, from any object, in any thread. It is safe to connect signals and slots across different threads, thanks to a mechanism called [queued connections](qt.html#ConnectionType-enum).

Another way to make code run in a separate thread, is to subclass QThread and reimplement [run](qthread.html#run)(). For example:

```
 class WorkerThread : public QThread
 {
     Q_OBJECT
     void run() override {
         QString result;
         /* ... here is the expensive or blocking operation ... */
         emit resultReady(result);
     }
 signals:
     void resultReady(const QString &s);
 };

 void MyObject::startWorkInAThread()
 {
     WorkerThread *workerThread = new WorkerThread(this);
     connect(workerThread, &WorkerThread::resultReady, this, &MyObject::handleResults);
     connect(workerThread, &WorkerThread::finished, workerThread, &QObject::deleteLater);
     workerThread->start();
 }
```

In that example, the thread will exit after the run function has returned. There will not be any event loop running in the thread unless you call [exec](qthread.html#exec)().

It is important to remember that a QThread instance [lives in](qobject.html#thread-affinity) the old thread that instantiated it, not in the new thread that calls [run](qthread.html#run)(). This means that all of QThread's queued slots and [invoked methods](qmetaobject.html#invokeMethod) will execute in the old thread. Thus, a developer who wishes to invoke slots in the new thread must use the worker-object approach; new slots should not be implemented directly into a subclassed QThread.

Unlike queued slots or invoked methods, methods called directly on the QThread object will execute in the thread that calls the method. When subclassing QThread, keep in mind that the constructor executes in the old thread while [run](qthread.html#run)() executes in the new thread. If a member variable is accessed from both functions, then the variable is accessed from two different threads. Check that it is safe to do so.

Note: Care must be taken when interacting with objects across different threads. See [Synchronizing Threads](../qtdoc/threads-synchronizing.html) for details. 

### Managing Threads

QThread will notifiy you via a signal when the thread is [started](qthread.html#started)() and [finished](qthread.html#finished)(), or you can use [isFinished](qthread.html#isFinished)() and [isRunning](qthread.html#isRunning)() to query the state of the thread.

You can stop the thread by calling [exit](qthread.html#exit)() or [quit](qthread.html#quit)(). In extreme cases, you may want to forcibly [terminate](qthread.html#terminate)() an executing thread. However, doing so is dangerous and discouraged. Please read the documentation for [terminate](qthread.html#terminate)() and [setTerminationEnabled](qthread.html#setTerminationEnabled)() for detailed information.

From Qt 4.8 onwards, it is possible to deallocate objects that live in a thread that has just ended, by connecting the [finished](qthread.html#finished)() signal to [QObject::deleteLater](qobject.html#deleteLater)().

Use [wait](qthread.html#wait)() to block the calling thread, until the other thread has finished execution (or until a specified time has passed).

QThread also provides static, platform independent sleep functions: [sleep](qthread.html#sleep)(), [msleep](qthread.html#msleep)(), and [usleep](qthread.html#usleep)() allow full second, millisecond, and microsecond resolution respectively. These functions were made public in Qt 5.0.

Note: [wait](qthread.html#wait)() and the [sleep](qthread.html#sleep)() functions should be unnecessary in general, since Qt is an event-driven framework. Instead of [wait](qthread.html#wait)(), consider listening for the [finished](qthread.html#finished)() signal. Instead of the [sleep](qthread.html#sleep)() functions, consider using [QTimer](qtimer.html).

The static functions [currentThreadId](qthread.html#currentThreadId)() and [currentThread](qthread.html#currentThread)() return identifiers for the currently executing thread. The former returns a platform specific ID for the thread; the latter returns a QThread pointer.

To choose the name that your thread will be given (as identified by the command ps -L on Linux, for example), you can call [setObjectName()](qobject.html#objectName-prop) before starting the thread. If you don't call [setObjectName()](qobject.html#objectName-prop), the name given to your thread will be the class name of the runtime type of your thread object (for example, "RenderThread" in the case of the [Mandelbrot Example](qtcore-threads-mandelbrot-example.html), as that is the name of the QThread subclass). Note that this is currently not available with release builds on Windows.

See also [Thread Support in Qt](../qtdoc/threads.html), [QThreadStorage](qthreadstorage.html), [Synchronizing Threads](../qtdoc/threads-synchronizing.html), [Mandelbrot Example](qtcore-threads-mandelbrot-example.html), [Semaphores Example](qtcore-threads-semaphores-example.html), and [Wait Conditions Example](qtcore-threads-waitconditions-example.html).

## Member Type Documentation

### enum QThread::Priority

This enum type indicates how the operating system should schedule newly created threads.



| Constant                      | Value | Description                                                  |
| ----------------------------- | ----- | ------------------------------------------------------------ |
| QThread::IdlePriority         | 0     | scheduled only when no other threads are running.            |
| QThread::LowestPriority       | 1     | scheduled less often than LowPriority.                       |
| QThread::LowPriority          | 2     | scheduled less often than NormalPriority.                    |
| QThread::NormalPriority       | 3     | the default priority of the operating system.                |
| QThread::HighPriority         | 4     | scheduled more often than NormalPriority.                    |
| QThread::HighestPriority      | 5     | scheduled more often than HighPriority.                      |
| QThread::TimeCriticalPriority | 6     | scheduled as often as possible.                              |
| QThread::InheritPriority      | 7     | use the same priority as the creating thread. This is the default. |

## Member Function Documentation

### QThread::QThread([QObject](qobject.html#QObject) **parent* = nullptr)

Constructs a new QThread to manage a new thread. The *parent* takes ownership of the QThread. The thread does not begin executing until [start](qthread.html#start)() is called.

See also [start](qthread.html#start)().

### [signal] void QThread::finished()

This signal is emitted from the associated thread right before it finishes executing.

When this signal is emitted, the event loop has already stopped running. No more events will be processed in the thread, except for deferred deletion events. This signal can be connected to [QObject::deleteLater](qobject.html#deleteLater)(), to free objects in that thread.

Note: If the associated thread was terminated using [terminate](qthread.html#terminate)(), it is undefined from which thread this signal is emitted.

Note: This is a private signal. It can be used in signal connections but cannot be emitted by the user.

See also [started](qthread.html#started)().

### [slot] void QThread::quit()

Tells the thread's event loop to exit with return code 0 (success). Equivalent to calling [QThread::exit](qthread.html#exit)(0).

This function does nothing if the thread does not have an event loop.

See also [exit](qthread.html#exit)() and [QEventLoop](qeventloop.html).

### [slot] void QThread::start([QThread::Priority](qthread.html#Priority-enum) *priority* = InheritPriority)

Begins execution of the thread by calling [run](qthread.html#run)(). The operating system will schedule the thread according to the *priority* parameter. If the thread is already running, this function does nothing.

The effect of the *priority* parameter is dependent on the operating system's scheduling policy. In particular, the *priority* will be ignored on systems that do not support thread priorities (such as on Linux, see the [sched_setscheduler](http://linux.die.net/man/2/sched_setscheduler) documentation for more details).

See also [run](qthread.html#run)() and [terminate](qthread.html#terminate)().

### [signal] void QThread::started()

This signal is emitted from the associated thread when it starts executing, before the [run](qthread.html#run)() function is called.

Note: This is a private signal. It can be used in signal connections but cannot be emitted by the user.

See also [finished](qthread.html#finished)().

### [slot] void QThread::terminate()

Terminates the execution of the thread. The thread may or may not be terminated immediately, depending on the operating system's scheduling policies. Use [QThread::wait](qthread.html#wait)() after terminate(), to be sure.

When the thread is terminated, all threads waiting for the thread to finish will be woken up.

Warning: This function is dangerous and its use is discouraged. The thread can be terminated at any point in its code path. Threads can be terminated while modifying data. There is no chance for the thread to clean up after itself, unlock any held mutexes, etc. In short, use this function only if absolutely necessary.

Termination can be explicitly enabled or disabled by calling [QThread::setTerminationEnabled](qthread.html#setTerminationEnabled)(). Calling this function while termination is disabled results in the termination being deferred, until termination is re-enabled. See the documentation of [QThread::setTerminationEnabled](qthread.html#setTerminationEnabled)() for more information.

See also [setTerminationEnabled](qthread.html#setTerminationEnabled)().

### [virtual] QThread::~QThread()

Destroys the [QThread](qthread.html).

Note that deleting a [QThread](qthread.html) object will not stop the execution of the thread it manages. Deleting a running [QThread](qthread.html) (i.e. [isFinished](qthread.html#isFinished)() returns false) will result in a program crash. Wait for the [finished](qthread.html#finished)() signal before deleting the [QThread](qthread.html).

### [static] template <typename Function, typename Args> [QThread](qthread.html#QThread) *QThread::create(Function &&*f*, Args &&... *args*)

Creates a new [QThread](qthread.html) object that will execute the function *f* with the arguments *args*.

The new thread is not started -- it must be started by an explicit call to [start](qthread.html#start)(). This allows you to connect to its signals, move QObjects to the thread, choose the new thread's priority and so on. The function *f* will be called in the new thread.

Returns the newly created [QThread](qthread.html) instance.

Note: the caller acquires ownership of the returned [QThread](qthread.html) instance.

Note: this function is only available when using C++17.

Warning: do not call [start](qthread.html#start)() on the returned [QThread](qthread.html) instance more than once; doing so will result in undefined behavior.

This function was introduced in Qt 5.10.

See also [start](qthread.html#start)().

### [static] template <typename Function> [QThread](qthread.html#QThread) *QThread::create(Function &&*f*)

Creates a new [QThread](qthread.html) object that will execute the function *f*.

The new thread is not started -- it must be started by an explicit call to [start](qthread.html#start)(). This allows you to connect to its signals, move QObjects to the thread, choose the new thread's priority and so on. The function *f* will be called in the new thread.

Returns the newly created [QThread](qthread.html) instance.

Note: the caller acquires ownership of the returned [QThread](qthread.html) instance.

Warning: do not call [start](qthread.html#start)() on the returned [QThread](qthread.html) instance more than once; doing so will result in undefined behavior.

This function was introduced in Qt 5.10.

See also [start](qthread.html#start)().

### [static] [QThread](qthread.html#QThread) *QThread::currentThread()

Returns a pointer to a [QThread](qthread.html) which manages the currently executing thread.

### [static] [Qt::HANDLE](qt.html#HANDLE-typedef) QThread::currentThreadId()

Returns the thread handle of the currently executing thread.

Warning: The handle returned by this function is used for internal purposes and should not be used in any application code.

Note: On Windows, this function returns the DWORD (Windows-Thread ID) returned by the Win32 function GetCurrentThreadId(), not the pseudo-HANDLE (Windows-Thread HANDLE) returned by the Win32 function GetCurrentThread().

### [override virtual] bool QThread::event([QEvent](qevent.html) **event*)

Reimplements: [QObject::event](qobject.html#event)(QEvent *e).

### [Q](qabstracteventdispatcher.html)AbstractEventDispatcher *QThread::eventDispatcher() const

Returns a pointer to the event dispatcher object for the thread. If no event dispatcher exists for the thread, this function returns nullptr.

This function was introduced in Qt 5.0.

See also [setEventDispatcher](qthread.html#setEventDispatcher)().

### [protected] int QThread::exec()

Enters the event loop and waits until [exit](qthread.html#exit)() is called, returning the value that was passed to [exit](qthread.html#exit)(). The value returned is 0 if [exit](qthread.html#exit)() is called via [quit](qthread.html#quit)().

This function is meant to be called from within [run](qthread.html#run)(). It is necessary to call this function to start event handling.

See also [quit](qthread.html#quit)() and [exit](qthread.html#exit)().

### void QThread::exit(int *returnCode* = 0)

Tells the thread's event loop to exit with a return code.

After calling this function, the thread leaves the event loop and returns from the call to [QEventLoop::exec](qeventloop.html#exec)(). The [QEventLoop::exec](qeventloop.html#exec)() function returns *returnCode*.

By convention, a *returnCode* of 0 means success, any non-zero value indicates an error.

Note that unlike the C library function of the same name, this function *does* return to the caller -- it is event processing that stops.

No QEventLoops will be started anymore in this thread until [QThread::exec](qthread.html#exec)() has been called again. If the eventloop in [QThread::exec](qthread.html#exec)() is not running then the next call to [QThread::exec](qthread.html#exec)() will also return immediately.

See also [quit](qthread.html#quit)() and [QEventLoop](qeventloop.html).

### [static] int QThread::idealThreadCount()

Returns the ideal number of threads that can be run on the system. This is done querying the number of processor cores, both real and logical, in the system. This function returns 1 if the number of processor cores could not be detected.

### bool QThread::isFinished() const

Returns true if the thread is finished; otherwise returns false.

See also [isRunning](qthread.html#isRunning)().

### bool QThread::isInterruptionRequested() const

Return true if the task running on this thread should be stopped. An interruption can be requested by [requestInterruption](qthread.html#requestInterruption)().

This function can be used to make long running tasks cleanly interruptible. Never checking or acting on the value returned by this function is safe, however it is advisable do so regularly in long running functions. Take care not to call it too often, to keep the overhead low.

```
 void long_task() {
      forever {
         if ( QThread::currentThread()->isInterruptionRequested() ) {
             return;
         }
     }
 }
```

This function was introduced in Qt 5.2.

See also [currentThread](qthread.html#currentThread)() and [requestInterruption](qthread.html#requestInterruption)().

### bool QThread::isRunning() const

Returns true if the thread is running; otherwise returns false.

See also [isFinished](qthread.html#isFinished)().

### int QThread::loopLevel() const

Returns the current event loop level for the thread.

Note: This can only be called within the thread itself, i.e. when it is the current thread.

This function was introduced in Qt 5.5.

### [static] void QThread::msleep(unsigned long *msecs*)

Forces the current thread to sleep for *msecs* milliseconds.

Avoid using this function if you need to wait for a given condition to change. Instead, connect a slot to the signal that indicates the change or use an event handler (see [QObject::event](qobject.html#event)()).

Note: This function does not guarantee accuracy. The application may sleep longer than *msecs* under heavy load conditions. Some OSes might round *msecs* up to 10 ms or 15 ms.

See also [sleep](qthread.html#sleep)() and [usleep](qthread.html#usleep)().

### [Q](qthread.html#Priority-enum)Thread::Priority QThread::priority() const

Returns the priority for a running thread. If the thread is not running, this function returns InheritPriority.

This function was introduced in Qt 4.1.

See also [Priority](qthread.html#Priority-enum), [setPriority](qthread.html#setPriority)(), and [start](qthread.html#start)().

### void QThread::requestInterruption()

Request the interruption of the thread. That request is advisory and it is up to code running on the thread to decide if and how it should act upon such request. This function does not stop any event loop running on the thread and does not terminate it in any way.

This function was introduced in Qt 5.2.

See also [isInterruptionRequested](qthread.html#isInterruptionRequested)().

### [virtual protected] void QThread::run()

The starting point for the thread. After calling [start](qthread.html#start)(), the newly created thread calls this function. The default implementation simply calls [exec](qthread.html#exec)().

You can reimplement this function to facilitate advanced thread management. Returning from this method will end the execution of the thread.

See also [start](qthread.html#start)() and [wait](qthread.html#wait)().

### void QThread::setEventDispatcher([QAbstractEventDispatcher](qabstracteventdispatcher.html) **eventDispatcher*)

Sets the event dispatcher for the thread to *eventDispatcher*. This is only possible as long as there is no event dispatcher installed for the thread yet. That is, before the thread has been started with [start](qthread.html#start)() or, in case of the main thread, before [QCoreApplication](qcoreapplication.html) has been instantiated. This method takes ownership of the object.

This function was introduced in Qt 5.0.

See also [eventDispatcher](qthread.html#eventDispatcher)().

### void QThread::setPriority([QThread::Priority](qthread.html#Priority-enum) *priority*)

This function sets the *priority* for a running thread. If the thread is not running, this function does nothing and returns immediately. Use [start](qthread.html#start)() to start a thread with a specific priority.

The *priority* argument can be any value in the QThread::Priority enum except for InheritPriority.

The effect of the *priority* parameter is dependent on the operating system's scheduling policy. In particular, the *priority* will be ignored on systems that do not support thread priorities (such as on Linux, see http://linux.die.net/man/2/sched_setscheduler for more details).

This function was introduced in Qt 4.1.

See also [Priority](qthread.html#Priority-enum), [priority](qthread.html#priority)(), and [start](qthread.html#start)().

### void QThread::setStackSize([uint](qtglobal.html#uint-typedef) *stackSize*)

Sets the maximum stack size for the thread to *stackSize*. If *stackSize* is greater than zero, the maximum stack size is set to *stackSize* bytes, otherwise the maximum stack size is automatically determined by the operating system.

Warning: Most operating systems place minimum and maximum limits on thread stack sizes. The thread will fail to start if the stack size is outside these limits.

See also [stackSize](qthread.html#stackSize)().

### [static protected] void QThread::setTerminationEnabled(bool *enabled* = true)

Enables or disables termination of the current thread based on the *enabled* parameter. The thread must have been started by [QThread](qthread.html).

When *enabled* is false, termination is disabled. Future calls to [QThread::terminate](qthread.html#terminate)() will return immediately without effect. Instead, the termination is deferred until termination is enabled.

When *enabled* is true, termination is enabled. Future calls to [QThread::terminate](qthread.html#terminate)() will terminate the thread normally. If termination has been deferred (i.e. [QThread::terminate](qthread.html#terminate)() was called with termination disabled), this function will terminate the calling thread *immediately*. Note that this function will not return in this case.

See also [terminate](qthread.html#terminate)().

### [static] void QThread::sleep(unsigned long *secs*)

Forces the current thread to sleep for *secs* seconds.

Avoid using this function if you need to wait for a given condition to change. Instead, connect a slot to the signal that indicates the change or use an event handler (see [QObject::event](qobject.html#event)()).

Note: This function does not guarantee accuracy. The application may sleep longer than *secs* under heavy load conditions.

See also [msleep](qthread.html#msleep)() and [usleep](qthread.html#usleep)().

### [u](qtglobal.html#uint-typedef)int QThread::stackSize() const

Returns the maximum stack size for the thread (if set with [setStackSize](qthread.html#setStackSize)()); otherwise returns zero.

See also [setStackSize](qthread.html#setStackSize)().

### [static] void QThread::usleep(unsigned long *usecs*)

Forces the current thread to sleep for *usecs* microseconds.

Avoid using this function if you need to wait for a given condition to change. Instead, connect a slot to the signal that indicates the change or use an event handler (see [QObject::event](qobject.html#event)()).

Note: This function does not guarantee accuracy. The application may sleep longer than *usecs* under heavy load conditions. Some OSes might round *usecs* up to 10 ms or 15 ms; on Windows, it will be rounded up to a multiple of 1 ms.

See also [sleep](qthread.html#sleep)() and [msleep](qthread.html#msleep)().

### bool QThread::wait([QDeadlineTimer](qdeadlinetimer.html) *deadline* = QDeadlineTimer(QDeadlineTimer::Forever))

Blocks the thread until either of these conditions is met:

- The thread associated with this [QThread](qthread.html) object has finished execution (i.e. when it returns from [run](qthread.html#run)()). This function will return true if the thread has finished. It also returns true if the thread has not been started yet.
- The *deadline* is reached. This function will return false if the deadline is reached.

A deadline timer set to QDeadlineTimer::Forever (the default) will never time out: in this case, the function only returns when the thread returns from [run](qthread.html#run)() or if the thread has not yet started.

This provides similar functionality to the POSIX pthread_join() function.

This function was introduced in Qt 5.15.

See also [sleep](qthread.html#sleep)() and [terminate](qthread.html#terminate)().

### bool QThread::wait(unsigned long *time*)

This is an overloaded function.

### [static] void QThread::yieldCurrentThread()

Yields execution of the current thread to another runnable thread, if any. Note that the operating system decides to which thread to switch. 

© 2020 The Qt Company Ltd. Documentation contributions included herein are the copyrights of their respective owners.
The documentation provided herein is licensed under the terms of the [GNU Free Documentation License version 1.3](http://www.gnu.org/licenses/fdl.html) as published by the Free Software Foundation.
Qt and respective logos are trademarks of The Qt Company Ltd. in Finland and/or other countries worldwide. All other trademarks are property of their respective owners. 