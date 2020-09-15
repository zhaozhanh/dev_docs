# jmap命令

## 帮助

命令：

```sh
jmap -h
```

结果示例:

```txt
    <none>               to print same info as Solaris pmap
    -heap                to print java heap summary
    -histo[:live]        to print histogram of java object heap; if the "live"
                         suboption is specified, only count live objects
    -clstats             to print class loader statistics
    -finalizerinfo       to print information on objects awaiting finalization
    -dump:<dump-options> to dump java heap in hprof binary format
                         dump-options:
                           live         dump only live objects; if not specified,
                                        all objects in the heap are dumped.
                           format=b     binary format
                           file=<file>  dump heap to <file>
                         Example: jmap -dump:live,format=b,file=heap.bin <pid>
    -F                   force. Use with -dump:<dump-options> <pid> or -histo
                         to force a heap dump or histogram when <pid> does not
                         respond. The "live" suboption is not supported
                         in this mode.
    -h | -help           to print this help message
    -J<flag>             to pass <flag> directly to the runtime system
```

## 查看堆配置参数及当前占用情况

命令:

```sh
jmap -heap PID
```

示例:

```txt
Attaching to process ID 32466, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.191-b12

using thread-local object allocation.
Parallel GC with 4 thread(s)

# 指java应用启动时设置的JVM参数。像最大使用内存大小，年老代，年青代，持久代大小等。
Heap Configuration:
   MinHeapFreeRatio         = 0
   MaxHeapFreeRatio         = 100
   MaxHeapSize              = 536870912 (512.0MB)
   NewSize                  = 268435456 (256.0MB)
   MaxNewSize               = 268435456 (256.0MB)
   OldSize                  = 268435456 (256.0MB)
   NewRatio                 = 2
   SurvivorRatio            = 8
   MetaspaceSize            = 134217728 (128.0MB)
   CompressedClassSpaceSize = 260046848 (248.0MB)
   MaxMetaspaceSize         = 268435456 (256.0MB)
   G1HeapRegionSize         = 0 (0.0MB)

# 当时的heap实际使用情况。包括新生代、老生代和持久代。
# 其中新生代包括：Eden区的大小、已使用大小、空闲大小及使用率。Survive区的From和To同样。
# 内存使用的堆积大多在老年代，内存池露始于此，所以要格外关心“Old Generation”。
Heap Usage:
PS Young Generation
Eden Space:
   capacity = 247463936 (236.0MB)
   used     = 42906528 (40.918853759765625MB)
   free     = 204557408 (195.08114624023438MB)
   17.338497355832892% used
From Space:
   capacity = 10485760 (10.0MB)
   used     = 7160544 (6.828826904296875MB)
   free     = 3325216 (3.171173095703125MB)
   68.28826904296875% used
To Space:
   capacity = 10485760 (10.0MB)
   used     = 0 (0.0MB)
   free     = 10485760 (10.0MB)
   0.0% used
PS Old Generation
   capacity = 268435456 (256.0MB)
   used     = 162261120 (154.7442626953125MB)
   free     = 106174336 (101.2557373046875MB)
   60.446977615356445% used
```

## 查看堆内对象占用空间大小，由高到低排序

命令:

```sh
jmap -histo PID
``

示例:

```txt
num     #instances         #bytes  class name
----------------------------------------------
   1:         12077       37306240     [I
   2:          8404        8913528       [B
   3:         55627        8311744      <constMethodKlass>
   4:         55627        7576152      <methodKlass>
   5:         35982        6771360     [C
   6:          4838        5536240     <constantPoolKlass>
   7:         88849        4696992    <symbolKlass>
   8:          4838        3735856    <instanceKlassKlass>
   9:          4024        3334976    <constantPoolCacheKlass>
  10:          4600        2201648    <methodDataKlass>
  11:         35011        1120352    java.lang.String
  12:          5286         549744    java.lang.Class
  13:          6509         441272    [S
  14:          7454         392128    [[I

其中关于I、B、C等的说明如下 Table 4.2.

BaseType Character	Type	Interpretation
B	byte	signed byte
C	char	Unicode character
D	double	double-precision floating-point value
F	float	single-precision floating-point value
I	int integer
J	long	long integer
L<classname>;	reference	an instance of class de><classname>de>
S	short	signed short
Z	boolean	de>truede> or de>falsede>
[	reference	one array dimension
```

## 堆转储

命令:

```sh
jmap -dump:format=b,file=filename java_pid

# 示例
jmap -dump:format=b,file=goods_heapdump.phrof 32466
```

> 可通过jvisualVM / jhat进行分析
