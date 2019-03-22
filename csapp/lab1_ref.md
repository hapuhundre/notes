
#### 目标
根据bits.c中的要求补全以下的函数：

``` c
int bitXor(int x, int y);

int tmin(void);

int isTmax(int x);

int allOddBits(int x);

int negate(int x);

int isAsciiDigit(int x);

int conditional(int x, int y, int z);

int isLessOrEqual(int x, int y);

int logicalNeg(int x);

int howManyBits(int x);

unsigned float_twice(unsigned uf);

unsigned float_i2f(int x);

int float_f2i(unsigned uf);
```

#### 编译与测试

在**32位Linux**下测试以上函数是否正确，指令如下：

* 编译：`make`  
* 测试：`./btest`


#### 结果

##### 1. bitXor
思路：根据离散数学的真值表即可得出结果：

``` c
/*
 * bitXor - x^y using only ~ and &
 *  Example: bitXor(4, 5) = 1
 *  Legal ops: ~ &
 *   Maxops: 14
 *  Rating: 1

 */

intbitXor(int x, int y) {

  return ~(~(~x&y)&~(x&~y));

}
```

##### 2. tmin
思路：最小值为0x8000 0000，由1左移31即可得到：

``` c
/*  tmin - return minimum two's complementinteger
 *  Legal ops: ! ~ & ^ | + << >>
 *   Maxops: 4
 *  Rating: 1

 */

inttmin(void) {

  return 1<<31;

}
```

##### 3. isTmax
思路：最大值为0x7fff ffff，加一会变为0x10000000，而此数加上本身后会变为0，本身加本身为0的数只有0和0x1000 0000，故而再将0xffffffff排除即可：

```
/*

 * isTmax - returns 1 if x is the maximum,two's complement number,

 *    and 0 otherwise

 *  Legal ops: ! ~ & ^ | +

 *   Maxops: 10

 *  Rating: 2

 */

intisTmax(int x) {

  return (!(x+1+x+1))&(!!(x+1));

}
```


##### 4. allOddBits
思路：只有所有奇数位为1的数（二进制），与0x5555 5555进行&预算才会得到0，故而需要得到0x5555 5555即可，将0x5分别左移4、8、16、24得到4个数，然后将这四个数和0x5相加即可得到0x5555 5555：

``` c
/*
 * allOddBits -return 1 if all odd-numbered bits in word set to 1
 *   Examples allOddBits(0xFFFFFFFD) = 0,allOddBits(0xAAAAAAAA) = 1
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 12
 *   Rating: 2
 */

int allOddBits(int x) {

  unsigned inta=85;

  unsigned intb=a+(a<<8)+(a<<16)+(a<<24);

  return !~(b|x);

}
```


##### 5. isAsciiDigit
思路：x需要>=’0’且<=’9’，将x与临界点作差，然后判断符号位的为0还是1即可：

``` c
/*
 * isAsciiDigit -return 1 if 0x30 <= x <= 0x39 (ASCII codes for characters '0' to '9')
 *   Example: isAsciiDigit(0x35) = 1.
 *            isAsciiDigit(0x3a) = 0.
 *            isAsciiDigit(0x05) = 0.
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 15
 *   Rating: 3
 */

int isAsciiDigit(int x) {

  return(!((x+~48+1)>>31))&!!((x+~58+1)>>31);

}
```

##### 6. conditional
思路：首先使用t=!x，当x为0时返回1，当x不为0时，返回0，根据题意得到( _ &y)|( _ &z)，首先空1，当x不为0，即t=0时，需要t转换为0xffffffff（-1），当t=1时，需要将t转换为0x0（0），，将t-1即可，得到空1为“!x+~1+1”，同理空2为“~!x+1”：

``` c
/*
 * conditional -same as x ? y : z
 *   Example: conditional(2,4,5) = 4
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 16
 *   Rating: 3
 */

int conditional(int x, int y, int z) {

  return ((!x+~1+1)&y)|((~!x+1)&z);

}
```

##### 7.  isLessOrEqual
思路：直接用y-x可能会超出int的表示范围，故而：

A、在x与y同号的情况下转换为p=y-x>=0，然后p符号位(p>>31）&1为0则返回1，符号位1则返回0；

B、 异号时，只要x>=0，就要返回0，否则返回1，由(x>>31)&1能达到该效果；

C、 c=a+b可作为x,y同号异号的判断。

``` c
/*
 * isLessOrEqual -if x <= y  then return 1, else return0
 *   Example: isLessOrEqual(4,5) = 1.
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 24
 *   Rating: 3
 */

int isLessOrEqual(int x, int y) {

  inta=x>>31;

  intb=y>>31;

  int c=a+b;

  int p=y+(~x+1);

  intq=!((p>>31)&1);

  intr=(c&(a&1))|((~c)&q);

  return r;

}
```


##### 8.  logicalNeg
思路：令y=~x+1，考虑x与y的符号位：

A.   当x为0时，两者符号位都为0；

B.    当x=0x8000 0000时，两者符号位都为1；

C.    否则，两者符号位为01或10；

D.   根据离散数学的真值表得出(~x)&(~y).

``` c
/*

 * logicalNeg -implement the ! operator, using all of
 *              the legal operators except !
 *   Examples: logicalNeg(3) = 0, logicalNeg(0) =1
 *   Legal ops: ~ & ^ | + << >>
 *   Max ops: 12
 *   Rating: 4

 */

int logicalNeg(int x) {

  return((~(~x+1)&~x)>>31)&1;

}
```

##### 9. howManyBits
思路：二分法，举例x=0000 1000 1001 0000 0000 0000 0000 0000说明：

A.     令y=x>>16=0000 1000 1001 0000，shift16=(!!y)<<4使用(!!y)如果y为0，则返回0，说明前16位都为0，shift16=0，否则!!y=1，shift16=16，使x=x>>shift16=10001001 0000;

B.      同理，令y=x>>8=0000 1000，shift8=(!!y)<<3使用(!!y)如果y为0，则返回0，说明前8位都为0，shift8=0，否则!!y=1，shift8=8，使x=x>>shift8=00001000;

C.      令y=x>>4=0000，shift4=(!!y)<<2使用(!!y)如果y为0，则返回0，说明前4位都为0，shift4=0，否则!!y=1，shift4=4，使x=x>>shift4=00001000;

D.     令y=x>>2=0000 10，shift2=(!!y)<<1使用(!!y)，如果y为0，则返回0，说明前2位（原始的x[27]x[26]）都为0，shift2=0，否则!!y=1，shift2=2，使x=x>>shift2=000010;

E.      令y=x>>1=0000 1，shift1=(!!y),使用(!!y)如果y为0，则返回0，说明前1位都为0，shift1=0，否则!!y=1，shift1=1，使x=x>>shift1=00001;

F.       然后令sum=2+ shift16+shift8+shift4+shift2+shift1即可；

G.     对于负数取反码即可，对于-1和0特殊考虑。

``` c
/* howManyBits - return the minimum number of bitsrequired to represent x in

 *             two's complement

 *  Examples: howManyBits(12) = 5

 *            howManyBits(298) = 10

 *            howManyBits(-5) = 4

 *            howManyBits(0)  = 1

 *            howManyBits(-1) = 1

 *            howManyBits(0x80000000) = 32

 *  Legal ops: ! ~ & ^ | + << >>

 *  Max ops: 90

 *  Rating: 4

 */

int howManyBits(int x) {

 intshift1,shift2,shift4,shift8,shift16;

 int sum;

 intt=((!x)<<31)>>31;//x为0时，t（二进制）全为1，x不为0时，全为1

 intt2=((!~x)<<31)>>31;//当x为-1时，t2全为1，否则，全为0

 //printf("x=%x\n",x);

 intop=x^((x>>31));//正数不变，负数取反

//printf("op=%x\n!!(op>>16)=%x\n",op,!!(op>>16));

 shift16=(!!(op>>16))<<4;//如果高十六位全为0，则0左移4位，不全为0，则1左移4（表示op要右移2^4位）位

 op=op>>shift16;

 shift8=(!!(op>>8))<<3;

 op=op>>shift8;

 shift4=(!!(op>>4))<<2;

 op=op>>shift4;

 shift2=(!!(op>>2))<<1;

 op=op>>shift2;

 shift1=(!!(op>>1));

 op=op>>shift1;

//printf("shift16=%x shift8=%x shift4=%x shift2=%xshift1=%x\n",shift16,shift8,shift4,shift2,shift1);

 sum=2+shift16+shift8+shift4+shift2+shift1;

//printf("t=%x sum=%x\n",t,sum);

 return(t2&1)|((~t2)&((t&1)|((~t)&sum)));

}
``` 

##### 10. float_twice
思路：分三种情况考虑

A.     当exp=0xff时，直接返回本身；

B.      当exp=0时，分两种情况考虑：

a)       当uf[22]=0时，直接即可frac<<1；

b)       当uf[22]=1时，exp++，然后frac<<1;

C．否则，exp++，然后分两种情况：

a)       当exp==0xff，令frac=0即可。

b)       否则不做处理。

``` c
/*

 * float_twice -Return bit-level equivalent of expression 2*f for

 *   floating point argument f.

 *   Both the argument and result are passed asunsigned int's, but

 *   they are to be interpreted as the bit-levelrepresentation of

 *   single-precision floating point values.

 *   When argument is NaN, return argument

 *   Legal ops: Any integer/unsigned operationsincl. ||, &&. also if, while

 *   Max ops: 30

 *   Rating: 4

 */

unsigned float_twice(unsigned uf) {

  //scanf("%x",&uf);

  unsigned intexp=0x7f800000&uf;

  unsigned intfrac=0x007fffff&uf;

  unsigned ints=0x80000000&uf;

 

 if(((~exp)&0x7f800000)==0){//exp=11111111

    //printf("%x\n",uf&0xff800000);

     return uf;

  }else{

     if(exp==0){

       if((0x00400000&uf)==0){

          frac=frac<<1;

        }else{

          exp=0x00800000;

          frac=frac<<1;

        }

     }else{

       exp=exp+0x00800000;

       if(((~exp)&0x7f800000)==0)

           frac=0;

     }

     //unsigned intx=(exp|frac|s);

     //printf("%x\n%x\n%x\n%x\n",s,exp,frac,x);

     return(exp|frac|s);

  }

}
``` 
 

##### 11. float_i2f
思路：首先我们需要知道int型的范围为-2^31~2^31-1主要有两种情况：

A.     用（二进制）科学计数法表示int型数时，尾数位数<=23，例如0x00008001，此时将0x8001左移24-16=8位得到frac，而exp则127+16-1；

B.      当尾数位数>23时，找到位数最末一位记作x[i]，然后对尾数的舍去分3种情况考虑，初始化c=0：

a)       当x[i-1]=1且x[i-2]、x[i-3]…x[0]都为0（即要偶端舍入情况），且x[i]=1,令c=1（此处frac若是全为1，则会导致frac+c超出范围，这是bug,当还是通过了）；

b)       当x[i-1]=1且x[i-2]、x[i-3]…x[0]不都为0，令c=1（与a存在同样的bug）;

c)       除a、b的情况，c=0；

C.      其他特殊情况再考虑一下就好了；


``` c
/*

 * float_i2f -Return bit-level equivalent of expression (float) x

 *   Result is returned as unsigned int, but

 *   it is to be interpreted as the bit-levelrepresentation of a

 *   single-precision floating point values.

 *   Legal ops: Any integer/unsigned operationsincl. ||, &&. also if, while

 *   Max ops: 30

 *   Rating: 4

 */

unsigned float_i2f(int x) {

 unsigned abs=x;

  unsigneds=0x80000000&x;

  unsignedtem=0x40000000;

  unsignedexp_sign=0;

  unsigned exp=0;

  unsigned frac;

  unsigned c=0;

  if(x==0)

       return x;

  elseif(x==0x80000000)

    return(s+(158<<23));

  else{

         if(x<0)

           abs=-x;

         while(1){

     if(tem&abs)

        break;

           tem=tem>>1;

     //printf("%x\n%x\n",tem,abs);

     exp_sign=exp_sign+1;

    }

  //  printf("exp_sign=%d\nx=%x\n",exp_sign,x);

   frac=(tem-1)&abs;

//printf("frac=%x\n",frac);

   if(31-1-exp_sign>23){//wei shu da yu 23wei de qingkuang

      inti=30-exp_sign-23;

     if((frac<<(31-(i-1)))==0x80000000){//ouduan quzhi

       if((frac&(1<<i))!=0)

          c=1;

      }

      elseif((frac&(1<<(i-1)))!=0)//dayu yiban jia 1

         c=1;

     frac=frac>>i;

    }

    else

     frac=frac<<(23-(31-exp_sign-1));


   exp=157-exp_sign;

   //printf("exp=%d\n s=%x\n",exp,s);

//printf("frac=%x\n",frac);

//printf("result=%x\n",(s+(exp<<23)+frac+c));

    return (s+(exp<<23)+frac+c);

  } 

}
```


##### 12. float_f2i
思路：exp为0，x=(-1)^s*0.frac*2^(-126)；否则x=(-1)^s*1.frac*2^(exp-127)分情况考虑：

A.     根据float转为int是向0舍入的情况，当exp=0或者exp<127(由exp-127<0得到)；

B.      令exp_sign=((exp>>23)-127)>=0，根据x=(-1)^s*1.frac*(exp-127)，

a)       exp_sign=0,x=1.x[22]x[21]…x[0]-->x=1;

b)       exp_sign=1,x=1x[22].x[21]…x[0]-->x=1x[22];

c)       exp_sign=2,x=1x[22]x[21].x[20]…x[0]-->x=1x[22]x[21];

d)      exp_sign=23,x=1x[22]x[21]x[20]…x[0]-->x=1x[22]x[21]…x[0]

e)       exp_sign=30,x=1x[22]x[21]x[20]…x[0]-->x=1x[22]x[21]…x[0]0…0(共31位)

f)        exp_sign=32,x=1x[22]x[21]x[20]…x[0]-->x=1x[22]x[21]…x[0]0…0(共32位)，此时，因为int为有符号，只有0x0000 00000能被表示出来，其他都是NAN;

C.      当exp_sign=((exp>>23)-127)>=33,NAN

``` c
/*

 * float_f2i -Return bit-level equivalent of expression (int) f

 *   for floating point argument f.

 *   Argument is passed as unsigned int, but

 *   it is to be interpreted as the bit-levelrepresentation of a

 *   single-precision floating point value.

 *   Anything out of range (including NaN andinfinity) should return

 *   0x80000000u.

 *   Legal ops: Any integer/unsigned operationsincl. ||, &&. also if, while

 *   Max ops: 30

 *   Rating: 4

 */

int float_f2i(unsigned uf) {

  intexp=0x7f800000&uf;
  unsigneds=0x80000000&uf;
  intfrac=0x007fffff&uf;
  intfrac2=frac+0x008fffff;
  int exp_sign=((exp>>23)-127);

//printf("uf=%x\n",uf);

 if(exp==0x7f800000)
    return 0x80000000u;

  else if(exp==0)
    return 0;

  else if(exp_sign<=-1&&exp_sign>=-126)
    return 0;

  else if(exp==31&&frac==0&&s!=0)
    return 0x80000000;

  else if(exp_sign>=31)
    return 0x80000000u;

  else if(exp_sign<=23)
   frac2 = frac2>>(23-exp);
   
  else
   frac2 = frac2<<(exp-23);

  if(s==0)
    return frac2;
  else
    return -frac2;

}
```
