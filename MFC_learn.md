## MFC (Microsoft Foundation Classes) Learning Note

- [MFC (Microsoft Foundation Classes) Learning Note](#mfc-microsoft-foundation-classes-learning-note)
	- [1. Basic concept](#1-basic-concept)
	- [2. WINAPI窗口程序](#2-winapi%e7%aa%97%e5%8f%a3%e7%a8%8b%e5%ba%8f)
	- [3. 一个MFC程序 (纯代码)](#3-%e4%b8%80%e4%b8%aamfc%e7%a8%8b%e5%ba%8f-%e7%ba%af%e4%bb%a3%e7%a0%81)
	- [4.根据向导创建工程](#4%e6%a0%b9%e6%8d%ae%e5%90%91%e5%af%bc%e5%88%9b%e5%bb%ba%e5%b7%a5%e7%a8%8b)
	- [5. 字符集](#5-%e5%ad%97%e7%ac%a6%e9%9b%86)
	- [6. 拓展](#6-%e6%8b%93%e5%b1%95)
	- [7. 对话框](#7-%e5%af%b9%e8%af%9d%e6%a1%86)
	- [8. 基于对话框(控件)编程](#8-%e5%9f%ba%e4%ba%8e%e5%af%b9%e8%af%9d%e6%a1%86%e6%8e%a7%e4%bb%b6%e7%bc%96%e7%a8%8b)
	- [9. 常用控件](#9-%e5%b8%b8%e7%94%a8%e6%8e%a7%e4%bb%b6)
	- [文档视图(二进制操作文件 CArchive)](#%e6%96%87%e6%a1%a3%e8%a7%86%e5%9b%be%e4%ba%8c%e8%bf%9b%e5%88%b6%e6%93%8d%e4%bd%9c%e6%96%87%e4%bb%b6-carchive)
	- [数据库编程](#%e6%95%b0%e6%8d%ae%e5%ba%93%e7%bc%96%e7%a8%8b)


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

1. 文档视图结构

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
    + ID: XXX_STATIC，静态ID，不响应任何消息(事件),不能关联控件变量

+ 按钮 CButton
    + Caption：修改现实的内容(属性里修改)
    + 处理消息 BN_CLICKED，用户点击按钮自动触发
        + 属性 -> 控制事件 -> 选择所需事件
        + 双击按钮，自动生成消息处理函数

+ 逃跑按钮(练习)
    + 自定义按钮类，继承于CButton
    + 选择类视图最开始的文件夹 -> 右击 -> 添加类 -> MFC -> MFC类
        + 处理鼠标移动消息 WM_MOUSEMOVE
        + 获取父窗口指针 CWnd::GetParent
        + 获取父窗口客户区域的范围 CWnd::GetClientRect
        + 获取按钮的范围 CWnd::GetWindowRect
        + 产生随机坐标 rand() %w
        + 移动按钮的位置 CWnd::MoveWindow

	+ 变量关联
    	+ 选中按钮 -> 右击 -> 添加变量 -> 变量类型：MyButton -> 变量：button
    	+ 最终，button和我们选中的按钮关联成功，操作button，相当于操作ui的按钮
    	+ 为按钮设置位图
        	+ 按钮属性：Bitmap -> True
        	+ 在对话框类中 OnInitDialog() 作如下处理
            	+ 创建位图模板
            	+ 创建位图对象 CBitmap
            	+ 加载位图资源 CBitmap::LoadBitmap
            	+ 按钮设置位图 CBitmap::SetBitmap
            	+ 获取位图大小 CBotmap::GetBitmap
            	+ 重新设置按钮大小(图片和按钮大小一致) CWnd::MoveWindow

+ 编辑框
    + 关联类别：Value，Control
        + Value :标准普通数据类型  ` CString str; `
        + 关联变量和控件数据的交互更新
            + 把编辑区的内容更新到str中 ` UpdaateData(TRUE) `
            + 把str的内容更新到编辑区中 ` UpdaateData(FALSE) `

		+ Control：控件类型：控件类型的对象即为ui上的控件

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
    	+ 属性设置：**顺序排放** Ctrl + D 查看
    	+ 同组第一个按钮 Group 设置为 True
    	+ 初始化单选框 CWnd::CheckRadioButton
    	+ 按钮是否按下 CWnd::IsDlgButtonChecked
  	+ 复选框
    	+ 常关联变量 BOOL UpdateData(TRUE), UpdateData(FALSE);
    	+ 设置按钮选择状态 CButton::SetCheck
    	+ 获取按钮选择状态 CButton::GetCheck

+ 列表框 CListBox
    + 给列表框添加一个字符串 CListBox::AddString
    + 选中列表列表框某一项，自动出发事件：LBN_SELCHANGE
        + 获取当前选中项 CListBox::GetCurse
        + 获取指定位置的内容 CListBox::GetText 
    + 删除指定位置的字符串 CListBox::DeleteString
    + 在指定位置插入字符串 CListBox::InsertString

+ 组合框(下拉框) CComboBox
    + 获取内容：CComboBox::GetLBText, 其他接口和CListBox 的用法几乎一样
    + 属性设置
        + data：设置内容，不同内容间用英文` ; `分隔
        + type

+ 滚动条(滑块) CScrollBar
    + 设置给定滚动条的最大和最小位置：CScrollBar::SetScrollRange
    + 获取一个滚动框的当前位置：CScrollBar::GetScrollPos
    + 设置一个滚动框的当前位置：CScrollBar::SetScrollPos
    + 处理滚动条的事件，不是在滚动条件控件本身处理，是在滚动条所属的父窗口处理（对话框类），处理信号：WM_HSCROLL
    + 滚动条位置关系
        ```
		switch(nSBCode) //判断滚动条的哪一部分
		{
			case SB_THUMBPOSITION: //滑块位置
				break;
			case SB_LINELEFT: //向左的箭头
				break;
			case SB_LINERIGHT: //向右的箭头
				break;
			case SB_PAGELEFT: //箭头和滑块之间左边
				break;
			case SB_PAGERIGHT: //箭头和滑块之间右边
				break;
			default:
				break;
		}
		``` 

+ 微调(旋转)按钮 SpinControl 的使用
    + 属性设置：
     
		Auto Buddy(自动伙伴) -> True

		Set buddy integer -> True
	
	+ 微调(旋转)按钮的顺序比伙伴大1(Ctrl + D 查看)，且编辑框序号不能为0


+ 列表视图控件 CListCtrl
    + 属性设置 view -> Report (报表形式)
    + 常用接口
        + 设置列表风格 CListCtrl::SetExtendedStyle

			LVS_EX_FULLROWSELECT：选择整行

			LVS_EX_GRIDLINES：网格方式

			具体有哪些风格，可通过MSDN查看
		
		+ 获取列表风格 CListCtrl::SetExtendedStyle

			具体有哪些风格，可通过MSDN查看

		+ 插入某列 CListCtrl::InsertColumn
		+ 字符串格式化
    		```
			CString str;
			str.Format(_("name%d), i);
			``` 
		+ 插入新项后，才能设置子项内容
    		+ 插入新项(确认第几行) CListCtrl::InsertItem
    		+ 设置子项内容(设置第几列) CListCtrl::SetItemText

+ 树视图控件 CTreeCtrl
    + 常用属性设置
        + has buttons -> true
        + has lines -> true
        + lines at root -> true
    + 写代码流程
        + 加载自定义图标
            + 获取应用程序对象指针 AfxGetApp()
            + 加载自定义图标 CWinApp::LoadIcon

+ 标签控件 CTabCtrl

### 文档视图(二进制操作文件 CArchive)

+ 文档视图结构
 
     文档类(CDocument): 存在加载(读写)数据

     视图类(CView): 显示和修改数据

	 + 单文档
    	 + 文档模板：把框架窗口、文档、视图关联在一起
    	 + 文档类(CDocument):

			OnNewDocument(), 第一次新建窗口调用，后面每次按"新建",自动调用此函数
			DeleteContents(), 做一些释放资源的操作，每次按“新建”时，新建前先调用此函数
		+ 框架类可认为是视图类的容器

	+ 各类相关访问
    	+ 在视图类，如何访问文档对象指针 CView::GetDocument
    	+ ` CDocument* GetDocument() cinst; `

+ 文档序列化(二进制操作文件)
		
	序列化：以二进制方式写文件

	反序列化：以二进制方式读文件

	+ 写文件
		+ 创建文件对象 CFile 
		+ 以写方式打开文件 CFile::Open
		+ 创建序列化对象，并且和文件关联在一起 CArchive
			+ CArchive::store 把数据保存到归档文件中。允许CFile写操作
		+ 往数据流写数据(相当于往文件写数据)

				ar << a << b << c 
		+  断开数据流和文件的关联 CArchive::Close
		+  关闭文件 CFile::Close

	+ 读文件
		+ 创建文件对象 CFile
		+ 以读方式打开文件 CFile::Open
		+ 创建序列化对象，并且和文件关联在一起 CArchive
		+ CArchive::store 把数据保存到归档文件中。允许CFile写操作 

+ 文档视图案例
    + 文档类自带序列化操作函数 Serialize()
    
	```
		void CCArchiveDoc::Serialize(CArchive& ar)
	{
		if (ar.IsStoring())
		{
			// TODO: 在此添加存储代码

			//按保存，调用此处
		}
		else
		{
			// TODO: 在此添加加载代码

			//打开文件按钮调用

		}
	}
	``` 
	+ 学生数据管理系统
    	+ 定义一个学生类 Student
    	+ 文档类存储数据，视图类修改和显示数据
        	+ 从尾部添加元素 CList::AddTail
        	+ 获得此列表尾部元素的位置 CList::GetTailPosition
        	+ 获取上一个元素 CList::GetPrev
        	+ 获取下一个元素 CList::GetNext
        	+ 获取首元素地址 CList::GetHeadPosition
        	+ 获取最后一个元素的位置 CList::GetTailPosition
        	+ 获取指定位置的元素 CList::GetAt
        	+ 移除头节点元素(并没有释放空间) CList::RemoveHead
    	+ 视图的基类是 CFormView
    	+ 重写文档类 DeleteContents(), 做一些释放资源的操作，每次按“新建”时，新建前先调用此函数
		
### 数据库编程

+ 准备工作
    + 安装MYSQL服务器
    + MYSQL odbc驱动(vs若是32位，则驱动必须为32位，否则vs里搜不到驱动)
	+ Navicat Premium 数据库管理工具安装

+ odbc层次图
    + odbc一套标准接口(内部通过sql语句操作数据库，用户就算不懂sql语句也可以借助odbc操作数据库)
    + 数据源(创建项目的时候用)

+ 如何创建数据源(MySQL只能是快照)
    + 快照(Snapshot)记录集：每次操作后重新查询才更新
    + 动态(Dynaset)记录集：每次操作自动更新(添加记录外)

+ 应用程序框架
    + CRecordset的子类，主要是对数据库进行相应操作
        + DoFieldExchange() 自动把数据库的字段和变量相关联
        + GetDefaultConnect() 获取数据库连接信息
        + GetDedaultSQL() 获取数据库连接的表
	+ CFormView的子类，显示数据库内容的视图
    	+ OnInitUpdate() 主要作初始化功能

+ 通过CRecordset类对数据库进行相应操作
    + 视图类头文件的创建CRecordset的子类对象
    + 视图类做 增删改查 操作
        + 打开数据库 CRecordset::Open
        + 查询记录 CRecordset::Requery
        + 定位当前记录为记录集中的下一个记录 CRecordset::MoveNext
        + 定位当前记录为记录集中上一个记录 CRecordset::MovePrev
        + 是否为最后一个记录的下一个 CRecordset::IsEOF
        + 是否为第一个记录的上一个 CRecordset::IsBOF
        + 移动到第一个记录 CRecordset::MoveFirst
        + 移动到最后一个记录 CRecordset::MoveLast
        + 添加空记录 CRecordset::AddNew
        + 如果记录集可修改 CRecordset::CanUpdate
        + 更新记录集 CRecordset::Update
        + 删除当前记录 CRecordset::Delete
        + 编辑当前记录 CRecordset::Edit
        + 过滤 CRecordset::m_strFilter
        + 排序 CRecordset::m_strSort(默认升序，降序加 desc)
    + 注意点
        + 移动记录集，注意越界处理
        + 移动记录集后，不要重新查询，重新查询相当于游标重新开始(回到最开始的位置)


