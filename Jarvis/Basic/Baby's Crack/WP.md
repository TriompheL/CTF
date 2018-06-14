Jarvis Basic Baby's Crack
==========================
1.	一道很基础的题
2.	打开有一个exe文件和一个.enc文件
3.	查看enc文件发现一行字符串
>jeihjiiklwjnk{ljj{kflghhj{ilk{k{kij{ihlgkfkhkwhhjgly
4.	运行crackme100.exe根据知道,这是一个用来加密的程序
5.	用IDA打开,main函数就是加密过程了.
```c
v8 = argc;
v9 = argv;
_main();
  if ( v8 <= 1 )       //v8是参数个数.如果程序没有传参数,v8=1 则提醒如何使用
  {
    printf("Usage: %s [FileName]\n", *v9); 
    printf(aFilename);
    exit(1);
  }
  File = fopen(v9[1], "rb+"); //二进制形式读写文件
  if ( File )   //文件不为空
  {
    v5 = fopen("tmp", "wb+");   //二进制形式追加
    while ( feof(File) == 0 )
    {
      v7 = fgetc(File);     //获取一个字符
      if ( v7 != -1 && v7 )
      {
        if ( v7 > 47 && v7 <= 96 )  //根据字符大小进行不同转换
        {
          v7 += 53;
        }
        else if ( v7 <= 46 )
        {
          v7 += v7 % 11;
        }
        else
        {
          v7 = 61 * (v7 / 61);
        }
        fputc(v7, v5);  //写字符
      }
    }
    fclose(v5);     //关闭文件
    fclose(File);
```
很简单.写个Python脚本
```python
key ="jeihjiiklwjnk{ljj{kflghhj{ilk{k{kij{ihlgkfkhkwhhjgly"
#key2位第一次输出的值,发现是16进制数,然后再进行了一次转换
key2 ="504354467B596F755F6172335F476F6F645F437261636B33527D"
ans =""
for j in key:
    for c in range(256):
        i = c
        if i >47 and i <=96:
            i += 53
        elif i <=46:
            i += i%11
        else:
            i = 61 * (i //61)
        if(j == chr(i)):
            ans += chr(c)
print(ans)

ans =''
for i in range(0,len(key2),2):
    ans+= chr(int(key2[i:i+2],16))
print(ans)
```
得到了falg
>PCTF{You_ar3_Good_Crack3R}
