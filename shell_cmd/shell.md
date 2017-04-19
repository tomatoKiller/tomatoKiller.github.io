## uniq命令

### 说明

这个命令用来删除重复的行，注意，重复的行必须**相邻**才能删除，因此这个命令常常和sort命令联合使用。先使用sort进行排序，让相同的行排在一起，然后用uniq进行去重。

### 选项

|  常用选项  |                含义                 |
| :----: | :-------------------------------: |
|   -c   |            显示每一行的重复次数             |
|   -d   |              只打印重复的行              |
|   -u   |              只打印非重复行              |
| -f num | 检测重复行时，忽略前num个field。不同field间用空格分割 |
| -s num |         检测重复行时，忽略前num个字符          |
|   -i   |               忽略大小写               |



### 示例

```shell
➜  test cat test.txt 
zic
aaa
bb
ccc
aaa
aaa
ccc
➜  test sort test.txt | uniq 
aaa
bb
ccc
zic
➜  test sort test.txt | uniq -c
   3 aaa
   1 bb
   2 ccc
   1 zic
➜  test sort test.txt | uniq -u
bb
zic
➜  test sort test.txt | uniq -s 2
aaa
bb
ccc
```

