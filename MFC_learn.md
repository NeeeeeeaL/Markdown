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
