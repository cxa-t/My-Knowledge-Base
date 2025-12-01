# Win32 API

平常我们运⾏起来的⿊框程序其实就是控制台程序

我们可以使⽤cmd命令来设置控制台窗⼝的⻓宽：设置控制台窗⼝的⼤⼩，30⾏，100列

```
mode con cols=100 lines=30
```

> 出师不利，刚开始就卡住了，cmd不能使用API命令？
>
> ![image-20251201173802383](C:\Users\CXA\AppData\Roaming\Typora\typora-user-images\image-20251201173802383.png)
>
> 先接着往下学吧，为什么别人电脑可以。

## 解决win11 mode con无效的过程

### ① 打开“真正的”旧版命令窗口

1. 同时按键盘上的 `Win + R`（Win 就是左下角窗户图标键）。

2. 弹出的小方框里输入

   ```
   cmd
   ```

3. 立刻同时按 `Ctrl + Shift + Enter`（**注意不是直接回车**）。
   如果系统问“是否允许更改”，点“是”。
   现在你看到的就是**纯黑色全屏 cmd**，不是 Windows Terminal。

### ② 先把字体调到最小（防止太大装不下）

1. 在黑色窗口**最上面蓝色条**→**右键**→选“**属性**”。
2. 上方点“**字体**”。
3. 左边选“**8×12**”或“**点阵 8**”→点“**确定**”。

> 这里还是选18吧，太小了看不清啊。

![image-20251201174508873](C:\Users\CXA\AppData\Roaming\Typora\typora-user-images\image-20251201174508873.png)

### ③ 把“缓冲区”和“窗口”一起改成 100×30

1. 还在刚才的“属性”窗口→上方点“**布局**”。
2. 会看到两行数字：
   - **屏幕缓冲区大小** → 宽、高
   - **窗口大小** → 宽、高
3. 把这两行**全部改成**：
   - **宽度 100**
   - **高度 30**

点右下角“**确定**”→关闭窗口。

​     

![image-20251201191721197](C:\Users\CXA\AppData\Roaming\Typora\typora-user-images\image-20251201191721197.png)

> 你看到的灰色，是因为 Windows 在你**手动拖过窗口边框**后，会自动把“由系统定位窗口”取消掉，并且把“窗口位置”锁死，导致**缓冲区宽度**那一栏也连带变灰。
> 只要**重新勾选**“由系统定位窗口”，所有数字会立刻变白，就能改了。

**窗口位置不需要改！**
只要勾了 **“由系统定位窗口”**，Windows 会自动把窗口放到屏幕左上角附近，**每次打开位置都一致**，省心又整齐。

- **左边(L)**、**上边(T)** 那两个数字保持灰色就行；
- 如果你把勾去掉，数字会变白，这时手动填了坐标后，窗口就**永远钉死**在那个像素点，换显示器或多屏时可能飞出可视区，反而麻烦。

所以：**让“由系统定位窗口”打着勾，窗口位置一律不动**，最省事！

现在就修改的设置就好了（也搞不太清楚）

## 验证

![image-20251201192655868](C:\Users\CXA\AppData\Roaming\Typora\typora-user-images\image-20251201192655868.png)

OK，现在就解决了。

# Win 32 API介绍

`Windows` 这个多作业系统除了协调应⽤程序的执⾏、分配内存、管理资源之外， 它同时也是⼀个很⼤

的服务中⼼，调⽤这个服务中⼼的各种服务（**每⼀种服务就是⼀个函数**），可以帮应⽤程序达到开启

视窗、描绘图形、使⽤周边设备等⽬的，由于这些函数服务的对象是应⽤程序(`Application`)， 所以便

称之为 `Application Programming Interface`，简称 `API` 函数。`WIN32 API`也就是`Microsoft Windows`

`32位平台`的应⽤程序编程接⼝。

# 控制台设置

![image-20251201193903856](C:\Users\CXA\AppData\Roaming\Typora\typora-user-images\image-20251201193903856.png)

这里呢，如果代码运行后是这个界面，那么就代表代码运行是在终端上，这时候需要进行设置一下，

![image-20251201193934391](C:\Users\CXA\AppData\Roaming\Typora\typora-user-images\image-20251201193934391.png)

修改成让`Windows`决定。

![image-20251201194234823](C:\Users\CXA\AppData\Roaming\Typora\typora-user-images\image-20251201194234823.png)

也可以直接选控制台主机。

![image-20251201194135171](C:\Users\CXA\AppData\Roaming\Typora\typora-user-images\image-20251201194135171.png)

吧背景颜色修改一下，便于区分。

---

# 正文开始

## 程序中设置控制台

`system`函数用来执行系统命令。

```c
system("mode con cols=30 lines=30");
```

![image-20251201194552055](C:\Users\CXA\AppData\Roaming\Typora\typora-user-images\image-20251201194552055.png)

```c
#include <stdio.h>
#include<windows.h>
int main()
{
	//设置控制台窗口大小
	system("mode con cols=100 lines=30");
	//设置控制台名称
	system("title 贪吃蛇");


	//设置控制台暂停 获取一个字符后继续
	system("pause");
	return 0;
}
```

![image-20251201194859503](C:\Users\CXA\AppData\Roaming\Typora\typora-user-images\image-20251201194859503.png)



## COORD控制台屏幕坐标

COORD 是Windows API中定义的⼀个结构体，表⽰⼀个字符在控制台屏幕上的坐标

```c
typedef struct _COORD {
  SHORT X;
  SHORT Y;
} COORD, *PCOORD;
```

![image-20251201195723831](C:\Users\CXA\AppData\Roaming\Typora\typora-user-images\image-20251201195723831.png)

![image-20251201195059166](C:\Users\CXA\AppData\Roaming\Typora\typora-user-images\image-20251201195059166.png)

屏幕上任何一个位置就是一个坐标。

怎么定义一个坐标呢？

```c
COORD pos = { 0,0 };
COORD pos = { 10,10 };
```

## GetStdHandle 函数

要控制控制台窗口，首先要获得这个窗口。

`GetStdHandle`是⼀个**Windows API函数**。它⽤于从⼀个特定的标准设备（标准输⼊、标准输出或标准错误）中取得⼀个**句柄**（⽤来标识不同设备的数值），使⽤这个句柄可以操作设备（也就是屏幕）。

> 句柄 其实是一个数字，只是叫句柄而已，简单理解为抓手。

函数定义（[飞机票](https://learn.microsoft.com/zh-cn/windows/console/getstdhandle)）

```c
HANDLE WINAPI GetStdHandle(
  _In_ DWORD nStdHandle
);
```

![image-20251201200328803](C:\Users\CXA\AppData\Roaming\Typora\typora-user-images\image-20251201200328803.png)

HANDLE是一种空指针变量。

![image-20251201200538679](C:\Users\CXA\AppData\Roaming\Typora\typora-user-images\image-20251201200538679.png)

## GetConsoleCursorInfo 函数（[飞机票](https://learn.microsoft.com/zh-cn/windows/console/getconsolecursorinfo)）

这里有光标一直在闪，如果贪吃蛇一直在跑的话，光标一直闪非常不方便，我们可以把光标隐藏起来。

![image-20251201200738028](C:\Users\CXA\AppData\Roaming\Typora\typora-user-images\image-20251201200738028.png)

检索有关指定控制台屏幕缓冲区的光标⼤⼩和可⻅性的信息。

```c
BOOL WINAPI GetConsoleCursorInfo(
  _In_  HANDLE               hConsoleOutput,
  _Out_ PCONSOLE_CURSOR_INFO lpConsoleCursorInfo
);
```

它的第一个参数就是刚才获取到的句柄。

第二个参数是指向 [**CONSOLE_CURSOR_INFO**](https://learn.microsoft.com/zh-cn/windows/console/console-cursor-info-str) 结构的指针，该结构接收有关控制台游标的信息。

### CONSOLE_CURSOR_INFO 结构

```c
typedef struct _CONSOLE_CURSOR_INFO {
  DWORD dwSize;
  BOOL  bVisible;
} CONSOLE_CURSOR_INFO, *PCONSOLE_CURSOR_INFO;
```

- dwSize，由光标填充的字符单元格的百分⽐。 此值介于1到100之间。 光标外观会变化，范围从完全填充单元格到单元底部的⽔平线条。

- bVisible，游标的可⻅性。 如果光标可⻅，则此成员为 TRUE。

示例代码：

```c
int main()
{
	//获取标准输出设备
	HANDLE houtput = GetStdHandle(STD_OUTPUT_HANDLE); 
	CONSOLE_CURSOR_INFO cursor_info = { 0 };

	GetConsoleCursorInfo(houtput, &cursor_info);

	system("pause");
	return 0;
}
```

![image-20251201202037804](C:\Users\CXA\AppData\Roaming\Typora\typora-user-images\image-20251201202037804.png)

这里的25表示看见的光标，只占到一个字符的25%。也可以直接隐藏，那就需要设置结构体另一个成员。

> 这里需要注意的是，不能只修改，不然将会不成功，而是设置 ---> 修改 ---> 再设置。

![image-20251201202610043](C:\Users\CXA\AppData\Roaming\Typora\typora-user-images\image-20251201202610043.png)

不是我没截，是真的没了。

## SetConsoleCursorPosition 函数

```c
BOOL WINAPI SetConsoleCursorPosition(
  _In_ HANDLE hConsoleOutput,
  _In_ COORD  dwCursorPosition
);
```

设置指定控制台屏幕缓冲区中的光标位置，将想要设置的坐标信息放在COORD类型的pos中，⽤SetConsoleCursorPosition函数将光标位置设置到指定的位置。

指定新光标位置（以字符为单位）的 [**COORD**](https://learn.microsoft.com/zh-cn/windows/console/coord-str) 结构。 坐标是屏幕缓冲区字符单元的列和行。 **坐标必须位于控制台屏幕缓冲区的边界以内。**

![image-20251201203910920](C:\Users\CXA\AppData\Roaming\Typora\typora-user-images\image-20251201203910920.png)

```c
 COORD pos = { 5, 5};
 HANDLE hOutput = NULL;
 //获取标准输出的句柄(⽤来标识不同设备的数值)
 hOutput = GetStdHandle(STD_OUTPUT_HANDLE);
 //设置标准输出上光标的位置为pos
 SetConsoleCursorPosition(hOutput, pos);
```

光标被定位到（5，5）。

### 设置光标位置函数set_pos

```c
void set_pos(short x, short y)
{
	//拿到标准输出设备句柄
	HANDLE houtput = GetStdHandle(STD_OUTPUT_HANDLE);
	//定义光标位置
	COORD pos = { x,y };
	//设置光标位置
	SetConsoleCursorPosition(houtput, pos);
}
```

## GetAsyncKeyState 函数

获取按键情况，GetAsyncKeyState的函数原型如下：

```c
SHORT GetAsyncKeyState( int vKey );
```

将键盘上每个键的[虚拟键值](https://learn.microsoft.com/en-us/windows/win32/inputdev/virtual-key-codes)传递给函数，函数通过返回值来分辨按键的状态。

![image-20251201205356966](C:\Users\CXA\AppData\Roaming\Typora\typora-user-images\image-20251201205356966.png)

`GetAsyncKeyState` 的返回值是short类型，在上⼀次调⽤ `GetAsyncKeyState` 函数后，如果返回的16位的short数据中，最⾼位是1，说明按键的状态是按下，如果最⾼是0，说明按键的状态是抬起；如果最低位被置为1则说明，该按键被按过，否则为0。

如果我们要判断⼀个键是否被按过，可以检测`GetAsyncKeyState`返回值的最低值是否为1.

```c
#define KEY_PRESS(VK) ( (GetAsyncKeyState(VK) & 0x1) ? 1 : 0 )
```

```c
int main()
{
	while (1)
	{
		if (KEY_PRESS(0x30))
			printf("0\n");
		else if(KEY_PRESS(0x31))
			printf("1\n");
		else if (KEY_PRESS(0x32))
			printf("2\n");
		else if (KEY_PRESS(0x33))
			printf("3\n");
		else if (KEY_PRESS(0x34))
			printf("4\n");
		else if (KEY_PRESS(0x35))
			printf("5\n");
		else if (KEY_PRESS(0x36))
			printf("6\n");
		else if (KEY_PRESS(0x37))
			printf("7\n");
		else if (KEY_PRESS(0x38))
			printf("8\n");
		else if (KEY_PRESS(0x39))
			printf("9\n");
		else if (KEY_PRESS(0x26))
			printf("上\n");
		else if (KEY_PRESS(0x25))
			printf("左\n");
		else if (KEY_PRESS(0x27))
			printf("右\n");
		else if (KEY_PRESS(0x28))
			printf("下\n");
	}
	return 0;
}
```

按键检测。

---

