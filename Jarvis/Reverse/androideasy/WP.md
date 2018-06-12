Jarvis 逆向androideasy
=======================

1.APK IDE打开,反编译APk成功,后找到class文件,用jd-gui打开,找到核心代码
```
if (MainActivity.this.check())	//调用check函数,返回值为true,输出正确
        {
          Toast.makeText(jdField_this, "You got the flag!", 1).show();
          return;
        }
```
2.然后再看check函数
给出要用的到数组
```
private byte[] s = { 113, 123, 118, 112, 108, 94, 99, 72, 38, 68, 72, 87, 89, 72, 36, 118, 100, 78, 72, 87, 121, 83, 101, 39, 62, 94, 62, 38, 107, 115, 106 };

public boolean check()
  {
byte[] arrayOfByte = this.editText.getText().toString().getBytes();
//将输入的字符串转换为字节与s字节数组中比较
    if (arrayOfByte.length != this.s.length) {
      return false;
    }
    int i = 0;
    for (;;)
    {
      if ((i >= this.s.length) || (i >= arrayOfByte.length)) {
        break label65;
      }
      if (this.s[i] != (arrayOfByte[i] ^ 0x17)) {	//逐个异或
        break;
      }
      i += 1;
    }
    label65:
    return true;
  }	
```

原理很简单,就是把输入的字符串逐个异或然后比较,
异或的逆运算还是异或,所以写脚本.
```
key =[ 113, 123, 118, 112, 108, 94, 99, 72, 38, 68,
       72, 87, 89, 72, 36, 118, 100, 78, 72, 87, 121,
       83, 101, 39, 62, 94, 62, 38, 107, 115, 106]

ans =''
for i  in range(0,len(key)):
    key[i] =key[i] ^0x17;
    ans += chr(key[i])
print(ans)
```
得到最后的flag
_flag{It_1S_@N_3asY_@nDr0)I)1|d}_
