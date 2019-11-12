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
