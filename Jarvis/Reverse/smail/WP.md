Jarvis 逆向 smail
===================
题目告诉我们smail是android的基础.大佬直接看smail文件
而我用工具**Smail2Java**得到java文件
```java
public class Crackme {
    private String str2 = "cGhyYWNrICBjdGYgMjAxNg==";
    
    public Crackme() {
        GetFlag("sSNnx1UKbYrA1+MOrdtDTA==");
    }
    
    private String GetFlag(String p1) {
        byte[] "content" = Base64.decode(p1.getBytes(), 0x0);
        String "kk" = new String(Base64.decode(str2.getBytes(), 0x0));
        System.out.println(decrypt("content", "kk"));
        return null;
    }
    
    private String decrypt(byte[] p1, String p2) {
        String "m" = 0x0;
        try {
            byte[] "keyStr" = p2.getBytes();
            SecretKeySpec "key" = new SecretKeySpec("keyStr", "AES");
            Cipher "cipher" = Cipher.getInstance("AES/ECB/NoPadding");
            "cipher".init(0x2, "key");
            byte[] "result" = "cipher".doFinal(p1);
            return "m";
        } catch(NoSuchPaddingException "e") {
            "e".printStackTrace();
        }
        return  "m";
    }
}
```
Python脚本
```python
import base64,binascii
from Crypto.Cipher import AES
key=base64.b64decode('cGhyYWNrICBjdGYgMjAxNg==')
pwd=base64.b64decode('sSNnx1UKbYrA1+MOrdtDTA==')
x = AES.new(key,AES.MODE_ECB)
print(x.decrypt(pwd))
```
得到flag
>PCTF{Sm4liRiver}
