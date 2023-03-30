# linux

### 常用命令

* ls

* touch

```shell
touch 1.txt
```

* head

实例：head 1.txt

```
-n 30  指定显示文件前面行数
```

* chmod

```shell
chmod 777 文件
```

* ping

```shell
ping ip+端口

ping 主机名/域名
```

* less

* more

* tail

* tar

```shell
tar -czvf test.tar.gz 1.txt 2.txt 文件目录 【压缩】

tar -xzvf test.tar.gz【解压】

tar -xzvf test.tar.gz -C ./qin/指定目录解压
```

* rm

使用：rm -rif

```shell
-d：直接把欲删除的目录的硬连接数据删除成0，删除该目录；
-f：强制删除文件或目录；
-i：删除已有文件或目录之前先询问用户；
-r或-R：递归处理，将指定目录下的所有文件与子目录一并处理；
--preserve-root：不对根目录进行递归操作；
-v：显示指令的详细执行过程。
```

* ln

说明：软连接

使用方式：

```shell
ln -s ./mysql ./mysql.link 软连接
ln 1.sh ./test/1.link 硬链接### 
```

* cp

用法： cp -a

```shell
cp aaa.txt ./bbb/b.txt  复制文件。


cp -a ./aaa ./bbb复制文件夹 
    说明：此参数的效果和同时指定"-dpR"参数相同；

-d：当复制符号连接时，把目标文件或目录也建立为符号连接，并指向与源文件或目录连接的原始文件或目录；
-r ./aaa ./bbb 说明：
-a/R 递归复制目录文件夹以及子文件
-p：说明-保留源文件或目录的属性；
```

* mv

```shell
mv 123.txt cangping 移动或者修改文件或者文件夹
```





sort排序

```shell
常用
-b, --ignore-leading-blanks    忽略开头的空白。
-f, --ignore-case              将小写字母作为大写字母考虑。
-h, --human-numeric-sort       根据存储容量排序(注意使用大写字母，例如：2K 1G)。
-n, --numeric-sort             根据数字排序。
-r, --reverse                  将结果倒序排列。
一般nr一起使用
-V, --version-sort             文本中(版本)数字的自然排序。
-t, --field-separator=SEP              使用SEP作为列的分隔符。
-k, --key=KEYDEF                       通过一个key排序；KEYDEF给出位置和类型。
一般-t -k一起使用 -t分割 -k指定列
默认情况下
sort -k 1 按照空格切割

-d, --dictionary-order         仅考虑空白、字母、数字。
-g, --general-numeric-sort     根据数字排序。
-i, --ignore-nonprinting       排除不可打印字符。
-M, --month-sort               按照非月份、一月、十二月的顺序排序。
-R, --random-sort              随机排序，但分组相同的行。
--random-source=FILE           从FILE中获取随机长度的字节。
--sort=WORD                    根据WORD排序，其中: general-numeric 等价于 -g，human-numeric 等价于 -h，month 等价于 -M，numeric 等价于 -n，random 等价于 -R，version 等价于 -V。
-o, --output=FILE                      将结果写入FILE而不是标准输出。
```

uniq去重

```shell
常用：
-c, --count                在每行开头增加重复次数。
-f, --skip-fields=N        跳过对前N个列的比较。
-w, --check-chars=N        只对每行前N个字符进行比较。
-s, --skip-chars=N         跳过对前N个字符的比较。
-d, --repeated             所有邻近的重复行只被打印一次。
-D                         所有邻近的重复行将全部打印。重复次数>=2

--all-repeated[=METHOD]    类似于 -D，但允许每组之间以空行分割。METHOD取值范围{none(默认)，prepend，separate}。
--group[=METHOD]           显示所有行，允许每组之间以空行分割。METHOD取值范围：{separate(默认)，prepend，append，both}。
-i, --ignore-case          忽略大小写的差异。
-u, --unique               只打印非邻近的重复行。
-z, --zero-terminated      设置行终止符为NUL（空），而不是换行符。
--help                     显示帮助信息并退出。
--version                  显示版本信息并退出。
```

wc统计

```shell
常用：
-c # 统计字节数，或--bytes或——chars：只显示Bytes数；。
-l # 统计行数，或——lines：只显示列数；。
-m # 统计字符数。这个标志不能与 -c 标志一起使用。
-w # 统计单词数，或——words：只显示字数。一个字被定义为由空白、跳格或换行字符分隔的字符串。
-L # 打印最长行的长度。


m-help     # 显示帮助信息
--version # 显示版本信息
```

### 性能监控

* netstat(重要)

使用：netstat -tnlp

```
-t, -–tcp    显示 TCP 传输协议的连线状况
-n, -–numeric    直接使用 IP 地址，而不通过域名服务器。
-l, -–listening    显示监控中的服务器的 Socket。
-p, -–programs    显示正在使用 Socket 的程序识别码和程序名称。
```

统计各种状态的tcp连接

```
netstat -tn | awk 'NR>2{print $NF}'| sort | uniq -c | sort -nr
```

​    问题：连接数受限，连接数太多。解决：查看netstat 对应端口的数量～

​    问题：系统参数制约， 解决：根据ulimit -a 查看对应参数，例如open-file数量过多

​    问题：如果CLOSE_WAIT TIME_WAIT 连接过多，代表程序有问题，解决：开发代码该释放的没有释放，该关闭的没有关闭

TCP连接状态：

```
ESTABLISHED 成功连接 The socket has an established connection 
SYN_SENT The socket is actively attempting to establish a connection
SYN_RECV A connection request has been received from the network.
FIN_WAIT1 The socket is closed, and the connection is shutting down.
FIN_WAIT2 Connection is closed, and the socket is waiting for a shutdown from the remote end
TIME_WAIT 主动关闭 The socket is waiting after close to handle packets still in the network
CLOSE The socket is not being used
CLOSE_WAIT 被动关闭 The remote end has shut down, waiting for the socket to close.
LISTEN The socket is listening for incoming connections
```

==ESTABLISHED 已建立的成功连接 套接字已建立连接(常用)==
SYN_SENT 套接字正在积极尝试建立连接
SYN_RECV 已从网络接收到连接请求。
FIN_WAIT1 套接字已关闭，连接正在关闭。
FIN_WAIT2 连接已关闭，套接字正在等待远程端关闭
==TIME_WAIT主动关闭 套接字在关闭后等待处理仍在网络中的数据包（常用==）
CLOSE 关闭未使用套接字
==CLOSE_WAIT 被动关闭 远程端已关闭，正在等待套接字关闭。（常用）==
LISTEN 侦听套接字正在侦听传入连接

* ps

```shell
unix 风格参数 ps -ef
bsd 风格参数 ps aux
gnu 风格参数 ps --pid pidlist


ps aux -m/-M

ps -o pid,ppid,psr,thcount,tid,cmd -M
```

![](https://raw.githubusercontent.com/qinjinghub2/picgo/main/test2022-11-11-20-09-27-image.png)

```
python demo.py 2 4 & /后台运行程序

jobs /列出后台程序

fg /切换停止的程序到前台

ctrl z /暂停程序到后台

bg /前台程序停止后，继续后台运行
```

![](https://raw.githubusercontent.com/qinjinghub2/picgo/main/test2022-11-11-20-16-34-image.png)

* top

默认 //每隔5秒显式所有进程的资源占用情况  
常用选项：

```
top -d 2 //每隔2秒显式所有进程的资源占用情况  
top -c //每隔5秒显式进程的资源占用情况，并显示进程的命令行参数(默认只有进程名)  
top -p 12345 -p 6789//每隔5秒显示pid是12345和pid是6789的两个进程的资源占用情况  
top -d 2 -c -p 123456 //每隔2秒显示pid是12345的进程的资源使用情况，并显式该进程启动的命令行参数
-b 批处理，一般持续输出并且管道到下一个命令
-n 输出次数
```

top信息：

![](https://raw.githubusercontent.com/qinjinghub2/picgo/main/test2022-11-11-22-27-39-image.png)

![](https://raw.githubusercontent.com/qinjinghub2/picgo/main/test2022-11-11-22-29-50-image.png)

1.cpu信息

| Cpu(s): 0.1 %us | 用户模式占用的 CPU 百分比                                         |
| --------------- | ------------------------------------------------------- |
| 0.1%sy          | 系统模式占用的 CPU 百分比                                         |
| 0.0%ni          | 改变过优先级的用户进程占用的 CPU 百分比                                  |
| 99.7%id         | 空闲 CPU 占用的 CPU 百分比                                      |
| 0.1%wa          | 等待输入/输出的进程占用的 CPU 百分比                                   |
| 0.0%hi          | 硬中断请求服务占用的 CPU 百分比                                      |
| 0.1%si          | 软中断请求服务占用的 CPU 百分比                                      |
| 0.0%st          | st（steal time）意为虚拟时间百分比，就是当有虚拟机时，虚拟 CPU 等待实际 CPU 的时间百分比 |

2.内存信息

| 内 容                | 说 明                                            |
| ------------------ | ---------------------------------------------- |
| Mem: 625344k total | 物理内存的总量，单位为KB                                  |
| 571504k used       | 己经使用的物理内存数量                                    |
| 53840k&ee          | 空闲的物理内存数量。我们使用的是虚拟机，共分配了 628MB内存，所以只有53MB的空闲内存 |
| 65800k buffers     | 作为缓冲的内存数量                                      |

![](https://raw.githubusercontent.com/qinjinghub2/picgo/main/test2022-11-11-20-24-14-image.png)

| VIRT                                                                                                                                        | RES（最重要）                                                                                                                                                                          | SHR                                                                                                                          |
| ------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| 虚拟内存                                                                                                                                        | 保留内存                                                                                                                                                                              | 共享内存                                                                                                                         |
| 1、进程“需要的”虚拟内存大小，包括进程使用的库、代码、数据，以及malloc、new分配的堆空间和分配的栈空间等；<br/>2、假如进程新申请10MB的内存，但实际只使用了1MB，那么它会增长10MB，而不是实际的1MB使用量。<br/>3、VIRT = SWAP + RES | 1、进程当前使用的内存大小，包括使用中的malloc、new分配的堆空间和分配的栈空间，但不包括swap out量；<br/>2、包含其他进程的共享；<br/>3、如果申请10MB的内存，实际使用1MB，它只增长1MB，与VIRT相反；<br/>4、关于库占用内存的情况，它只统计加载的库文件所占内存大小。<br/>5、RES = CODE + DATA | 1、除了自身进程的共享内存，也包括其他进程的共享内存；<br/>2、虽然进程只使用了几个共享库的函数，但它包含了整个共享库的大小；<br/>3、计算某个进程所占的物理内存大小公式：RES – SHR；<br/>4、swap out后，它将会降下来。 |

linux进程内存

| 类型                  | 内存名称               | 解释                  |
| ------------------- | ------------------ | ------------------- |
| A：                  | 进程本身内存             | 进程直接操纵的内存           |
| B：                  | 共享内存               | 程序共用的api，dll资源      |
| C:                  | 应用创建时申请的内存         | 需要的时候直接占用，不需要时也在占用。 |
|                     |                    |                     |
| Vss 虚拟内存   （名称：VSZ） | 进程本身内存+共享内存+申请的内存  | 进程内部内存              |
| Rss 保留内存            | 进程本身内存+共享内存        | 进程实际占用内存（B累加计算）     |
| Uss 进程本身内存          | 进程本身内存=A           | 程序自用内存              |
| Pss 实际进程本身内存        | 进程本身内存+共享内存/实际运行程序 | 进程实际占用内存（B/实际运行程序）  |

​          

注意：ps命令cpu占用率不准确，内存统计准确 

![](https://raw.githubusercontent.com/qinjinghub2/picgo/main/test2022-11-11-21-35-04-image.png)

查看进程内存信息，根据内存大小排序

```
ps -e -o uid,pid,ppid,pcpu,pmem,rss,vsz,comm --sort -%mem | head -10
```

free

使用方式：free

常用选项 :    -h   说明：按照人类理解的内存展示数据

![](https://raw.githubusercontent.com/qinjinghub2/picgo/main/test2022-11-11-19-34-41-image.png)

```
选项:

-b，——bytes以字节形式显示输出

-k，——kilo以千字节显示输出

-m，——mega显示兆字节的输出

-g，——giga显示以gb为单位的输出

——tera显示以tb为单位的输出

-h，——human显示人类可读输出

——si使用1000的幂而不是1024
```

**性能相关：**

查看cpu信息

cat /proc/cpuinfo

```
processor:cpu核心0，1，2，3
model name：cpu品牌
```

查看内存信息

cat /proc/meminfo

```shell
MemTotal:        8117336 kB 总内存
MemFree:         1036576 kB 剩余内存
MemAvailable:    5766724 kB 可用内存
Buffers:          156448 kB 缓冲区
Cached:          3956112 kB 缓存区
SwapCached:            0 kB 交换区
Active:          3067252 kB
Inactive:        2396532 kB
Active(anon):    1339624 kB
Inactive(anon):    16896 kB
Active(file):    1727628 kB
Inactive(file):  2379636 kB
Unevictable:          16 kB
Mlocked:              16 kB
SwapTotal:        969960 kB
SwapFree:         969960 kB
Dirty:               216 kB
Writeback:             0 kB
AnonPages:       1351236 kB
Mapped:           466704 kB
Shmem:             18604 kB
KReclaimable:     928680 kB
Slab:            1139732 kB
SReclaimable:     928680 kB
SUnreclaim:       211052 kB
KernelStack:       15456 kB
PageTables:        47700 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:     5028628 kB
Committed_AS:    6635420 kB
VmallocTotal:   34359738367 kB
VmallocUsed:       53924 kB
VmallocChunk:          0 kB
Percpu:           142848 kB
HardwareCorrupted:     0 kB
AnonHugePages:         0 kB
ShmemHugePages:        0 kB
ShmemPmdMapped:        0 kB
FileHugePages:         0 kB
FilePmdMapped:         0 kB
CmaTotal:              0 kB
CmaFree:               0 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
Hugetlb:               0 kB
DirectMap4k:      542528 kB
DirectMap2M:     7845888 kB
DirectMap1G:     2097152 kB
```

# linux三剑客

### grep

1.内容检索

```shell
获取行 `grep pattern file`

获取内容 `grep -o pattern file`
grep -o '[0-9]*1$' /tmp/1

获取上下文 `grep -A -B -C pattern file 
A1向后取一行 B1向前取一行 C1 前后个取一行    grep -C1 'oem' ./fotiaoqiang.log -C后面需要数字制定显示行数
```

2.文件检索

```shell
递归搜索       grep pattern -r dir/
grep aa -r ./

展示匹配文件内容  grep -h 111 /tmp/1
grep -h  aa -r ./

只展示匹配文件名 grep -l 111 /tmp/1
grep -l aa -r ./
```

3.范围约束

```shell
忽略大小写 grep -i pattern file

不显示匹配的行 grep -v pattern file

使用扩展正则表达式 grep -E pattern file

文件范围和目录范围约束 grep 111 -r /tmp/demo/ --include "11*" 限定指定的文件或目录
```

![image-20230207155514309](https://raw.githubusercontent.com/qinjinghub2/picgo/main/testimage-20230207155514309.png)

4.进程过滤

```shell
ps -ef | grep ssh
  503  2507     1   0 29 821  ??         0:00.08 /usr/bin/ssh-agent
  503 50022 11154   0  8:11下午 ttys002    0:00.00 grep ssh

ps -ef | grep ssh  | grep -v grep
  503  2507     1   0 29 821  ??         0:00.08 /usr/bin/ssh-agent
```

### awk

- 语法 `awk 'pattern{action}'`

```shell
- awk 是 linux 下的一个命令，同时也是一种语言解析引擎
- awk 具备完整的编程特性。比如执行命令，网络请求等
- 精通 awk，是一个 linux 工作者的必备技能
```

语法参数：

```shell
--awk 上下文变量

```shell
- 开始 BEGIN 结束 END
- 行数 NR
- 字段与字段数 $1 $2 .. $NF NF  #NF为字段数  $NF为最后一列字段
- 整行 $0
- 字段分隔符 FS
- 输出数据的字段分隔符 OFS
- 记录分隔符 RS
- 输出字段的行分隔符 ORS
```



--字段变量用法

```shell
- -F 参数指定字段分隔符，可以用|指定多个- 多分隔符 -F ‘<|>’ cat 
awk.txt | awk -F"[,./]" '{print $1,$2,$3,$4,$5}' 

- BEGIN{FS="_"} 也可以表示分隔符
- $0 代表当前的记录
- $1 代表第一个字段
- $N 代表第 N 个字段
- $NF 代表最后一个字段
- $(NF-1) 代表倒数第二个字段
```

--pattern 表达式

```shell
- 正则匹配 `$1~/pattern/` `/pattern/`
- 比较表达式 `$2>2` `$1=="b"`
```

--action 行为表达式

```shell
打印 {print $0} {print $2}
赋值 {$1="abc"}
处理函数
原始内容 $0
更新后内容 {$1=$1;print $0}
```

```
#### awk pattern 匹配表达式案例

```shell
- 开始和结束 awk 'BEGIN{}END{}'


- 正则匹配
  - 整行匹配 awk '/Running/'
  - 字段匹配 awk '$2~/xxx/'


- 行数表达式
  - 取第二行 awk 'NR==2'
  - 去掉第一行 awk 'NR>1'


- 区间选择
  - awk '/aa/,/bb/'
  - awk 'NR==2,/1/'
```

实用执行样例

```shell
单行转多行

```shell
echo 1:2:3 | awk 'BEGIN{RS=":"}{print $0}'
1
2
3
```

多行变单行

```shell
echo '1
2
3' | awk 'BEGIN{RS="";FS="\n";OFS=":"}{$1=$1;print $0}'
1:2:3


echo '1
2
3' | awk 'BEGIN{ORS=":"}{$1=$1;print $0}'
1:2:3:
```

计算平均数

```shell
echo '1,10
2,20
3,30' | awk 'BEGIN{total=0;FS=","}{total+=$2}END{print total/NR}'
20
```

awk 的词典结构 array

```shell
- array 是稀疏矩阵，类似 python 的词典类型
- 统计多家机构的营业额
- 统计多家机构的营业额平均值


echo 'a, 1, 10
a, 2, 20
a, 3, 30
b, 1, 5
b, 2, 6
b, 3, 7' | awk '{data[$1]+=$3}
END{for(k in data) print k,data[k]}'
a, 60
b, 18

echo 'a, 1, 10
a, 2, 20
a, 3, 30
b, 1, 5
b, 2, 6
b, 3, 7' | awk '{data[$1]+=$3;count[$1]+=1;}
END{for(k in data) print k,data[k]/count[k]}'
a, 20
b, 6
```

### sed

语法：

```shell
sed [options] [pattern]X[action] file(s)
```



options[选项]

```shell
-i 直接修改源文件
-e 表达式
-n或--quiet或——silent：仅显示script处理后的结果；sed -n ‘2p’ 打印第二行
-r 正则匹配 匹配使用
-E 扩展表达式
–debug 调试
```



sed pattern 表达式

```shell

匹配操作，字符串操作
s 查找替换：s/REGEXP/REPLACEMENT/[FLAGS] 

行操作，改变的是行
p 打印，通畅结合-n 参数：sed -n ‘2p’
d 删除              sed ‘1,2d’ 删除前两行
a 追加              sed ‘/1/a kk’ 向后追加
c 改变              sed '/1/c rr' 当前改变
i 插入内容到匹配行之前 sed '/1/i rr' 插入之前
```



```
sed -n '1p' 1.log  

指定显示行数范围 30,35
sed -n '1,3p' 1.log

正则匹配 /pattern/
sed -n '/1$/p' 1.log 
a, 1, 11
a, 1, 21
b, 3, 31
c, 4, 41
a, 1, 51
b, 2, 61
c, 3, 71

sed -n '//p' 1.log 

a, 1, 11
a, 1, 21
a, 1, 51
区间匹配 //,//
sed -n '/11/,/61/p' 1.log 
a, 1, 11
b, 2, 12
c, 3, 13

a, 1, 21
b, 3, 31
c, 4, 41

a, 1, 51
b, 2, 61

a 在匹配内容后追加
echo abc |sed '$a 123'
abc
123

echo abc |sed '/b/i 123'
123
abc

i在匹配前加入一行内容
echo abc |sed '/$/i 123'
123
abc
```



* 行数操作

```shell
打印特定行 sed -n 2p
删除最后一行 sed $d
```

 

* s 表达式

```shell
s 表示替换
s 后面的追加字符可以为任意字符
g 表示全局匹配
& 表示匹配内容


echo a:b:c | sed 's/:/123&/'
a123:b:c

echo a:b:c | sed 's/:/&123/'
a:123b:c

echo a:b:c |sed 's/:/12&3/g'
a12:3b12:3c

echo a:b:c | sed 's#:#|#g'
a|b|c
```

* 反向引用

```shell
- 使用()对数据进行分组
- 使用\1 \2 反向引用分组
echo 0 1 2 3 4 | sed -E 's#([1-3]) ([1-3]) ([1-3])#\3 \2 \1#'
0 3 2 1 4
```

**实际运用：**

* 统计aliyundun的性能

```shell
perf_get ()   
{
top -b -d 1 -n 20 |grep --line-buffered AliYunDun$ |awk 'B{print "cpu","mem"}{cpu+=$9;mem+=$10;print $9,$10}END{print ""; print cpu/NR,mem/NR}'
}
```

* 统计连接网络链接情况

链接所有的端口和对应的tcp连接状态，找出他们的连接总数吧

```shell
netstat -atnp | awk '$4~/:22$/' |awk '{print $4,$5,$7}'|awk 'BEGIN{FS=" rint $2}' |awk 'BEGIN{FS=":"}{print $1}'|sort -nr|uniq |wc -l
```
