## MFC (Microsoft Foundation Classes) Learning Note

### 1. Basic concept

SDK：(Software Development Kit) 软件开发工具包

JDK：(Java Development Kit) Java软件开发工具包

API：(Application Programming Interface) 应用程序接口

WINAPI：Windows平台下的系统调用，windows.h，调用系统提供的特殊接口，得到系统资源

窗口：父窗口和子窗口，客户区和非客户区

句柄：结构体类型，窗口句柄：HWND，图标句柄：HICO

消息队列

消息

窗口过程函数

` WinMain() // WINAPI 入口地址 `

### 2. WINAPI窗口程序

1. 定义入口函数WinMain()
2. 创建一个窗口
    + 设计窗口类WNDCLASS (给成员变量赋值)
    + 注册窗口类 ()
    + 显示和更新窗口
3. 消息循环
4. 窗口过程函数

### 3. 一个MFC程序 (纯代码)

1. 应用程序类 CWinApp
2. 框架类 CFrameWnd

头文件：

```
#pragma once //使头文件只被编译一次
#include <afxwin.h>

class MyApp: public CWinApp 
{
public:

	//MFC程序的入口地址
	virtual BOOL InitInstance();

private:
};

class MyFrame: public CFrameWnd
{
public:
	MyFrame();

private:

};

```

源文件：

```
#include "mfc.h"

//有且只有一个的应用程序类对象
MyApp myApp;

//程序的入口地址
BOOL MyApp::InitInstance()
{
	/*
	1. 创建框架类对象
	2. 显示窗口
	3. 更新窗口
	4. 保存框架类对象指针
	*/

	//1. 创建框架类对象
	MyFrame *frame = new MyFrame;//构造函数

	//2. 显示窗口
	frame->ShowWindow(SW_SHOWNORMAL);

	//3. 更新窗口
	frame->UpdateWindow();

	//4. 保存框架类对象指针
	m_pMainWnd = frame;



	return true;
}

MyFrame::MyFrame()
{
	//CWnd类的成员函数 CWnd::Create
	//CFrameWnd 继承于 CWnd

	Create(NULL, TEXT("MFC"));
}
```
方法：

1. 有且只有一个的应用程序类对象
2. 在程序入口函数实现功能 ` InitInstance() `
   + 给框架类对象动态分配空间(自动调用它的构造函数)
     + 框架类对象MyFrame对象构造函数里创建窗口 ` CWnd::Create `
   + 框架类对象显示窗口 ` CWnd::ShowWindow `
   + 框架类对象刷新窗口 ` CWnd::UpdateWindow `
   + 保存框架类对象指针 ` m_pMainWnd `，不保存的话，窗口会一直闪

加上事件处理：

消息映射
1. 在所操作的类中，声明消息映射宏

    ` DECLARE_MESSAGE_MAP() `
2. 对应的.cpp定义宏

```
BEGIN_MESSAGE_MAP(MyFrame, CFrameWnd) //参数：派生类，基类名
	ON_WM_LBUTTONDOWN() //消息映射入口
END_MESSAGE_MAP()
```
3. 对应类中，消息处理函数声明
4. 消息处理函数书定义

### 4.根据向导创建工程

1. 文档试图结构

文档：是一个类，专门存储数据

视图：是一个类，显示和修改数据

框架类：一个容器，装了视图

2. 几个重要函数：

应用程序类 CWinApp：` InitInstance() `

框架类 CFrameWnd:

+ ` PreCreateWindow() ` 创建窗口之前调用
+ ` OnCreate() ` 创建窗口后，触发WM_CREATE，它是WM_CREATE消息的处理函数
+ ` OnDraw() `绘图，WM_PAINT消息处理函数` OnPaint() `内部调用` OnDraw() `

***` OnPaint() `和` OnDraw() `同时存在，只有` OnPaint() `有效***

1. 事件的添加和删除

+ 框架和视图的区别

	+ 框架相当于容器，容器里放着视图
	+ 视图相当于壁纸

### 5. 字符集

ANSI 多字节，单字节

` char p[] = "abcdet"; //一个字符一个字节 `

 unicode 宽字节，一个字符两个字节

 ` TCHAR *P = L"abc"; //一个字符两个字节 `

 ` wcslen(p); `

 MFC:

 ` TCHAR `：自动适应字节(条件编译)

用以下两种方式显示字符：
```
TEXT("NeaL")
_T("NeaL")
```

### 6. 拓展

` afx_xxx `：全局函数，不属于某个类特有的

` xxxEx `; ` xxxW `：拓展函数，与原函数功能大致相同

MFC命名规范：

类、函数命名：首字母大写

```
class MyClass
{

};

void SetName()
{

};

//形参
isFlag
isPressTest

//成员变量
m_xxxx
m_hWnd

```

### 7. 对话框

+ 模态对话框(当其显示时，程序会暂停执行，直到关闭这个模态对话框后，才能继续执行程序中其他任务)
  + 资源视图 -> Dialog -> 右击 -> 插入 Dialog
  + 创建对话框对象 CDialog
  + 以模态方式运行 CDialog::DoModal

+ 非模态对话框(当其显示时，允许转而执行程序中其他任务，而不用关闭这个对话框)
  + 资源视图 -> Dialog -> 右击 -> 插入 Dialog
  + 创建对话框对象，需要在.h的地方声明为成员变量 CDialog
  + 创建对话框(在构造函数或OnCreate()，目的是只创建一次) CDialog::Create
  + 显示窗口 CWnd::ShowWindow

+ 自定义对话框类(重要)
  + 资源视图 -> Dialog -> 右击 -> 插入 Dialog
  + 点击对话框模板 -> 右击 -> 添加类
  + 多出来一个自定义的类，.h类中有个枚举和对话框关联 ` enum { IDO = IDD_DIALOG2}; `

### 8. 基于对话框(控件)编程

基于对话框应用程序框架

+ 应用程序类：继承于CWinApp

	` InitInstance() //程序入口地址 ` 
+ 对话框类：继承于CDialogEx
  
	` OninitDialog() //对话框的初始化工作 `

	` DoDataExchange() //控件和变量的关联和交换 `

### 9. 常用控件

+ 静态控件 CStatic：显示文字信息
  + Caption：修改现实的内容(属性里修改)
  + ID: XXX_STATIC，静态ID，不响应任何消息(事件)

+ 按钮 CButton
  + Caption：修改现实的内容(属性里修改)
  + 处理消息 BN_CLICKED，用户点击按钮自动触发
    + 属性 -> 控制事件 -> 选择所需事件
    + 双击按钮，自动生成消息处理函数
  + 常用属性设置
    + Number -> True 只能输入数字
    + Password -> True 密码模式
    + Want return -> True 接收回车键，自动换行，只有在多行模式下，才能换行
    + Multiline -> True 多行模式
    + Auto VScoll -> True , Vertical Scroll -> True 当垂直方向字符太多，自动出现滚动条
    + Read Only -> True 只读
  + 复制小案例(关联 control：控件类型，只能关联一次)
    + 获取编辑区内容：CWnd::GetWindowText
    + 设置编辑区内容：CWnd::SetWindowText
    + 关闭对话框窗口
     
		` CDialog::OnOK(); `

		` CDialog::OnCancel(); `

+ 单选框、复选框(特殊的CButton，没有单选框，复选框类型)

	+ 单选框
    	+ 
