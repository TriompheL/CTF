##Jarvis 的逆向题 DD-Hello##
先用file 查看了一下文件
```
$ file 1.Hello
1.Hello: Mach-O 64-bit x86_64 executable
```
64位 x86_64系列,直接用IDA打开,里面东西很少就几个函数,我把所有函数都分析了一下
```
	__int64 start()
{
  printf("Welcome\n");
  sub_100000C90();
  return 0LL;
}
```
Start函数,输出Welcome,调用函数sub_100000C90();返回0
接着看函数sub_100000C90();
```
int sub_100000C90()
{
  return printf("This is a dummy function\n");
}
```
输出字符串,这是一个假函数,然后返回打印字符的个数.
我们发现这两个函数都没有起作用.
```
int sub_100000CE0()
{
  int result; // eax
  signed int v1; // [rsp+1Ch] [rbp-14h]
  int v2; // [rsp+24h] [rbp-Ch]

  v2 = ((unsigned __int64)((char *)start - (char *)sub_100000C90) >> 2) ^ byte_100001040[0];
  result = sub_100000DE0();		//这个函数是不用管的
  if ( !(result & 1) )				//核心代码开始
  {
    v1 = 0;
    while ( v1 < 55 )
    {
      byte_100001040[v1] -= 2;
      byte_100001040[v1] ^= v2;
      ++v1;
      ++v2;
    }
    result = printf("\nFinal output is %s\n", &byte_100001040[1]);
  }
  return result;
}
```
我们可以看到最后输出是,说明这才是解题关键.
主要有两个关键部分
>v2 = ((unsigned __int64)((char *)start - (char *)sub_100000C90) >> 2) ^ byte_100001040[0];
取strat函数和sub_100000C90函数的地址进行运算,最后把值存到v2中
```
if ( !(result & 1) )				
  {
    v1 = 0;
    while ( v1 < 55 )
    {
      byte_100001040[v1] -= 2;
      byte_100001040[v1] ^= v2;
      ++v1;
      ++v2;
    }
```
byte_100001040 字符串已经给出.
 

最后脚本
```
key = [0x41, 0x10, 0x11, 0x11, 0x1B, 0x0A, 0x64, 0x67,
       0x6A, 0x68, 0x62, 0x68, 0x6E, 0x67, 0x68, 0x6B,
       0x62, 0x3D, 0x65, 0x6A, 0x6A, 0x3D, 0x68, 0x4,
       0x5, 0x8, 0x3, 0x2, 0x2, 0x55, 0x8, 0x5D, 0x61,
       0x55, 0x0A, 0x5F, 0x0D, 0x5D, 0x61, 0x32, 0x17,
       0x1D, 0x19, 0x1F, 0x18, 0x20, 0x4, 0x2, 0x12, 0x16,
       0x1E, 0x54, 0x20, 0x13, 0x14, 0x0, 0x0]
ans =''
k =((0x0000000100000CB0 - 0x0000000100000C90) >> 2) ^ key[0]
i=0
while i < 55:
    key[i] -= 2
    key[i] ^= k
    i += 1
    k += 1

for c in key:
    ans += chr(c)

print(ans[1:-2])
```
得到最后的flag
**DDCTF-5943293119a845e9bbdbde5a369c1f50@didichuxing.com**

