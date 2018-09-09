| 函数名 | 解释 |
| :--- | :--- |
| active\_count\(\) | 返回当前活动的Thread对象数量。 |
| current\_thread\(\) | 返回该函数调用者所在的线程的Thread对象。 |
| enumerate\(\) | 列出当前所有活动的Thread对象 |
| local\(\) | 返回local对象，用于保存线程本地的数据。应该保证此对象在每个线程中是唯一的。 |
| setprofile\(func\) | 设置一个配置文件函数，用于已创建的所有线程。func在每个线程开始运行之前被传递给sys.setprofile\(\)函数。 |
| settrace\(func\) | 设置一个跟踪函数，用于已创建的所有线程。func在每个线程开始运行之前被传递给sys.settrace\(\)函数。 |
| stack\_size\(size\) | 返回创建新线程时使用的栈大小。可选的整数参数size表示创建新线程时使用的栈大小。size的值可以是32768（32kb）或更大，而且是4096（4kb）的倍数，这样可移植性更好。如果系统上不支持此操作，将引发ThreadError异常。 |



