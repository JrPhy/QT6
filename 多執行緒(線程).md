在 QT 中會需要持續去監聽某事件，此時整個程式就像當掉一樣，所以會把該監聽的事件另外開一個執行緒。在 QT 中有兩種方法去執行新的執行緒，重載 run() 與 moveToThread()，並 ```#include<QThread>```。

## 1. 重載 run()
使用此方法要繼承 QThread 然後再去重載 run()，就可以執行一個無窮迴圈，但此種方法只能接收信號，無法發送信號。此種適合用來作與畫面無關的事情。
```cpp
// Mythread.h
class Mythread : public QThread
{
    Q_OBJECT
private:
    void run()  {
        qDebug() << "hello from Mythread";
    }
};

// main.cpp
int main(int argc, char *argv[]) {
    QCoreApplication app(argc, argv);
    Mythread thread;
    thread.start();
    qDebug() << "hello from GUI thread ";
    thread.wait();  // do not exit before the thread is completed!
    return 0;
}
```
啟用新的線程使用 start() 而非 run()，跟 c++ 中的 thread 有些許不同。其中 wait() 則是會用來等待 thread 中的函數執行結束才會往下跑，如果要暴力關掉的話可以使用 terminate()。

## 2. moveToThread
[程式碼可參考這影片](https://www.bilibili.com/video/BV1Eu4114793/?spm_id_from=333.880.my_history.page.click)。此函數是將線程移到 Object 的子類中，從其他父類繼承的就沒辦法。此種方法可以藉由 connect 收發訊號，但無法執行無窮迴圈。
```
// SubThread.h
class SubThread : public QThread
{
    Q_OBJECT
private:
    void Run() { // 非覆寫，名字不同
        int cnt = 10;
        while(cnt) { // 非無窮迴圈
            emit SendData(cnt--);
            QThread::sleep(1);
        }
        QThread::currentThread()->quit();
        // 迴圈結束後就結束線程
    }
};

// MyTHread.cpp
#include "mythread.h"
#include "./ui_mythread.h"
#include "subthread.h"
MyTHread::MyTHread(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MyTHread)
{
    ui->setupUi(this);
    QThread *thread = new QThread;
    SubThread *st = new SubThread;
    st->moveToThread(thread);
    connect(thread, &QThread::finished,
            st, &SubThread::deleteLater);

    connect(thread, &QThread::started,
            st, &SubThread::Run);

    connect(st, &SubThread::SendData,
            this, &MyTHread::RecvData);
}

MyTHread::~MyTHread() {
    delete ui;
    thread->deleteLater();
}

void MyTHread::on_btnStart_clicked() {
    thread->start();
    ui->btnStart->setDisabled(true);
}

void MyTHread::RecvData(int cd) {
    ui->textBrowser->append(QString::number(cd));
}
```
可以看到在建構時就把 MyTHread 中的 thread 移到 SubThread 中，然後在 SubThread 中會發訊號給 MyTHread，接收到訊號後會將數值印到 textBrowser 上。發送完訊號後並沒有馬上刪除子線程，因為此時線程還在畫面中，也有可能再次被使用，刪除就有可能造成程式 crash，所以是在關掉畫面後才將子線程刪除。

