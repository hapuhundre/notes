# CS:APP Lab Notes 1

标签（空格分隔）： 计算机系统

---

[TOC]

## Lab 1:Data Lab 数值操作
### 1.1 位运算
**要求** 
在bits.c文件中编写以下函数，不能使用循环和条件语句，运算符只能使用```! ~ & ^ | ^ + << >>```
![image_1bd8siivg1qtc1ecs1s2betoepbm.png-35.9kB][1]

**代码与注释**
``` c
/* 
 * bitAnd - x&y using only ~ and | 
 *   Example: bitAnd(6, 5) = 4
 *   Legal ops: ~ |
 *   Max ops: 8
 *   Rating: 1
 */
int bitAnd(int x, int y) {
    return ~((~x) | (~y));
}
/* 
 * getByte - Extract byte n from word x
 *   Bytes numbered from 0 (LSB) to 3 (MSB)
 *   Examples: getByte(0x12345678,1) = 0x56
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 6
 *   Rating: 2
 *   提取第n位，最低位为0，Examples对应的是0x78,第1位对应的是0x56
 *   最低位(LSB)为0，最高位(MSB)为3
 *   参考练习题：2.12 p35
 */
int getByte(int x, int n) {

  return (x >>(n<<3))&0xFF;

}
/* 
 * logicalShift - shift x to the right by n, using a logical shift
 *   Can assume that 0 <= n <= 31
 *   Examples: logicalShift(0x87654321,4) = 0x08765432
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 20
 *   Rating: 3
 *   逻辑右移，
 *   不管最高位是否为1，左边补0
 */
int logicalShift(int x, int n) {
  /* 思路:
  *  最高位为1111 1101这种情况，将0x1移位到1101最前面1这里
  *  将0x1>>31取反,得到0x7FFF FFFF
  *  再将其右移动n-1位，其实是移动32-n,将最高位对齐
  *  取逻辑与操作，1111和0000消去，而1101和1111与操作，没有变化
  */
  return (x>>n) & ((~(0x1<<31))>>n<<1)
}
/*
 * bitCount - returns count of number of 1's in word
 *   Examples: bitCount(5) = 2, bitCount(7) = 3
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 40
 *   Rating: 4
 *   
 */
int bitCount(int x) {
  return 2;
}
/* 
 * bang - Compute !x without using !
 *   Examples: bang(3) = 0, bang(0) = 1
 *   Legal ops: ~ & ^ | + << >>
 *   Max ops: 12
 *   Rating: 4 
 */
int bang(int x) {
  return 2;
}
```
**小结**
说实话，第一部分的实验还是很tricky的，尤其是后三个函数，网上的参考资料很多，但是还是要多多理解位操作的含义与意图

### 1.2 
### 1.3 浮点运算
### 1.4 实验总结


***参考资料***
1. [深入理解计算机系统（CSAPP）实验二 datalab-handout][2]
2. [CSAPP深入理解计算机系统实验datalab解析][3]
3. [【不周山之读厚 CSAPP】I Data Lab][4]
  


  [1]: http://static.zybuluo.com/hapuhundre/hnuhg8v41ksm9xzmj15azs5y/image_1bd8siivg1qtc1ecs1s2betoepbm.png
  [2]: http://www.cnblogs.com/tenlee/p/4951639.html
  [3]: http://blog.csdn.net/u014124795/article/details/38471797
  [4]: http://wdxtub.com/2016/04/16/thick-csapp-lab-1/
