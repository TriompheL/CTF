Jarvis 逆向 stheasy
====================

1.	先用软件Exeinfo 查壳. 得到主要信息**ELF文件,32位**
2.	使用IDA打开
3.	找到main函数,按F5.先改掉那些难以看懂的函数名
``` c
int __cdecl main()
{
  char s; // [esp+10h] [ebp-110h]

  printf("Input flag:");
  fun2(&s, 0x100u);      //从缓冲区获取字符串,赋值给长度为256的字符数组s
  if ( fun1(&s) )		//调用fun1函数,如果返回true,则输出flag is right
    puts("Flag is right.");
  else
    puts("Flag is wrong.");
  return 0;
}
```
4.	我们从主函数可以看出主要是fun1函数在起作用

``` c
	int __cdecl sub_8048630(char *s)
{
  size_t v1; // eax
  int v3; // edx

  if ( s )                                      // s不为空
  {
    v1 = strlen(s);
    if ( v1 )                                   // 字符串s长度不为0
    {
      if ( v1 == 29 )                           // 字符串长度必须为29
      {
        v3 = 0;
        while ( s[v3] == string_s1[(string_s2[v3] / 3u - 2)] )
        {
          if ( ++v3 == 29 )
            return 1;
        }
      }
    }
  }
  return 0;
}
```
5.	从中可以看出,原理很简单就是将string_2的值计算后的结果当做string_s1的索引
然后与我们输入的字符串一一比较
6.	写下python脚本

``` python
string_s1="k2j9Gh}AgfY4ds-a6QW1#k5ER_T[cvLbV7nOm3ZeX{CMt8SZo]U"
string_s2 =[0x48,0x5D,0x8D,0x24,0x84,0x27,0x99,0x9F,0x54,
            0x18,0x1E,0x69,0x7E,0x33,0x15,0x72,0x8D,0x33,
            0x24,0x63,0x21,0x54,0x0C,0x78,0x78,0x78,0x78,
            0x78,0x1B,0x0,0x0]

ans =''

for i in range(0,29):
    j =string_s2[i] // 3 - 2
    ans += string_s1[j-1]

print(ans)
```
7.	得到flag
8.	**kctf{YoU_hAVe-GOt-fLg_233333}**
