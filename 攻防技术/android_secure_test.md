# 安卓安全测试

App的安全问题包含两方面：

- app是否具有攻击行为，即是否有信息窃取、恶意扣费、远程控制等高危险的行为；
- app是否具有防御能力，即是否能够抵御逆向入侵、劫持篡改等攻击行为；
  以下围绕app的安全防御能力开展测试，介绍测试要求和测试方法。

普遍存在的安全问题：

- apk反编译后暴力关键业务代码
- 网络通信明文传输关键业务数据
- 受限服务接口未经授权可以远程任意访问
- 本地敏感数据未加密可被其他app任意读取

安全防护技术：防篡改、防调试、防注入、数据加密、安全通信等

Android app编程语言：主要为java
Android 四大组建：

- Activity
- Service
- Broadcast Receiver
- Content Provider

Android app的编译过程：

```java
// 创建Test.java

public class Test{
    public static void main(String[] xargs){
        Test test = new Test();
        System.out.println(test.add(1,2));

    }
    public int add(int a,int b){
        return a + b;
    }
}
```

将java原文件编译为class文件，语法`javac javafilename`,执行命令`javac Test.java` 会生成Test.class
查看java字节码文件内容，语法 `javap -c classfilename`,执行命令`javap -c Test.class`，执行结果如下：

```java
Compiled from "Test.java"
public class Test {
  public Test();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public static void main(java.lang.String[]);
    Code:
       0: new           #7                  // class Test
       3: dup
       4: invokespecial #9                  // Method "<init>":()V
       7: astore_1
       8: getstatic     #10                 // Field java/lang/System.out:Ljava/io/PrintStream;
      11: aload_1
      12: iconst_1
      13: iconst_2
      14: invokevirtual #16                 // Method add:(II)I
      17: invokevirtual #20                 // Method java/io/PrintStream.println:(I)V
      20: return

  public int add(int, int);
    Code:
       0: iload_1
       1: iload_2
       2: iadd
       3: ireturn
}
```

 android上的app使用自带的`Dalvik`虚拟机运行，并不使用标准的java虚拟机。两个虚拟机的指令集不同，通过javac编译生成的class文件无法在`Dalvik`虚拟机上运行，需要使用`dx`命令将所有的class文件和jar包转换成符合`Dalvik`字节码格式的`classes.dex`文件。

- `dx`命令位置：`\sdk\build-tools\`路径下
- 命令格式：`dx --dex --output=classes.dex Test.class`
## nuxus 5 手机root
1. 系统设置中找到版本号，多次点击打开开发者模式。
2. 授权手机允许电脑进行adb链接
```bash
# 手机授权之前 adb devices 的输出
adb devices     
List of devices attached
00f14aabf15dd26b        unauthorized
# 手机授权之后 adb devices 的输出
adb devices     
List of devices attached
00f14aabf15dd26b        device

```
3. 开发这选项中找到OEM解锁，打开选项，这个也就是刷机中常说的bootloader锁。
4. 进入bootloader界面
```bash
# 通过以下命令进入 或 关机后使用电源键和音量减键 进入
adb reboot bootloader 
```
5. 解锁OME
```bash
fastboot oem unlock
# 通过音量键 选择yes
# bootloader界面 显示 unlocked 标识已解锁 ，locked 标识未解锁

```
6. 官方镜像站点下载nexus 5 的刷机文件
手机设置中确认系统版本号
```markdown
型号：nexus 5x
android 版本：8.0.0
版本号：OPR4.170623.006
```
版本代号确认：
```http
# 确认网址：
https://source.android.com/docs/setup/reference/build-numbers?hl=zh-cn#source-code-tags-and-builds

# 代号
Oreo
```

6. 