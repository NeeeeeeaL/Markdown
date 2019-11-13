# C++ Learning Note

## QT介绍

+ 析构函数
    ```
    //构造函数，主要做初始化工作
    MainWindow::MainWindow(QWidget *parent) :
        QMainWindow(parent),
        ui(new Ui::MainWindow)
        {
            int *p;
            p = NULL;
            p = (int *)malloc(sizeof(int));
            *p = 10;

        }

    //析构函数，对象结束前（窗口关闭前），自动调用
    MainWindow::~MainWindow()
    {
        delete ui;

        if(p != NULL)
        {
            free(p);
            p =NULL;
        }
    }
    ```

    `printf` 不能打印 QString 类型，如需要在命令行打印输出，则需要用到 qDebug()函数
    ```
    #include <QDebug>
        qDebug();
    ```

+ 如果需要封装函数，函数内部要用到Qt一个类中的对象（如ui的控件），此函数必须为类中的函数，且要在类中声明，类外定义，加上作用域(`void MainWindow::test()`)。

+ **字符编码问题**
  
    + 给Qt