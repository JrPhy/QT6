在寫 QT 前需要先對環境做建置，qt 雖然是有自己的 qmake，但個人習慣使用 [cmake](https://github.com/JrPhy/C_tutorial/blob/main/CH13-CMake.md)，目前大部分的專案也是使用 cmake。安裝好後在建立一個 qt6 專案資料夾，並建立 CMakeList.txt 輸入以下內容
```
cmake_minimum_required(VERSION 3.10)

project(Begin)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_AUTOMOC ON) # qt6 選項
set(CMAKE_AUTORCC ON) # qt6 選項
set(CMAKE_AUTOUIC ON) # qt6 選項

find_package(Qt6 COMPONENTS Widgets REQUIRED)

add_executable(qt_example main.cpp)
target_link_libraries(qt_example Qt::Widgets)
```
然後建立 build 資料夾，並在下方建立一個 main.cpp 輸入以下程式碼。
## 1. 第一支 QT 程式
```cpp
#include <QApplication>
#include <QWidget>
#include <QLabel>

int main(int argc, char *argv[]) {
    QApplication app(argc, argv); // 程式初始化
    QWidget w; // qt 視窗物件
    w.setWindowTitle("My 1st QT");
    w.resize(600, 400);
    w.show(); // 顯示視窗
    QLabel label("hello"); // 視窗文字
    label.setWindowTitle("My 2nd QT");
    label.show();
    label.resize(600, 400);
    return app.exec();
}
```
最後執行```cmake ../``` 然後 ```make``` 就可以編譯出執行檔。執行後會看到兩個視窗，一個是由 QWidget 所產生的，標題為```My 1st QT```。另一個是由 QLabel 所產生的，標題為```My 2nd QT```，且視窗內有文字```hello```。

## 2. 視窗排版
在 QT 中常用的視窗類有兩個，QWidget 與 QLabel。這兩個類是在 QT 中常見的 UI 類，QWidget 為窗口的父類，QLabel 為繼承 QWidget 的子類。若要在視窗內給文字就需要用 QLabel，否則可以只用 QWidget。在 QWidget 中有 setLayout 函數可以來做排版，官方也提供了很多範例，這邊用比較常見的水平跟鉛直來做。需要引入```#include <QHBoxLayout>``` 與 ```#include <QVBoxLayout>``` 

```cpp
#include <QApplication>
#include <QWidget>
#include <QLabel>
#include <QHBoxLayout> // 新標頭檔
#include <QVBoxLayout> // 新標頭檔
int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    QWidget w;

    QVBoxLayout* vLayout = new QVBoxLayout; // 垂直排版
    w.setLayout(vLayout);

    QHBoxLayout* hLayout1 = new QHBoxLayout; // 水平排版一號
    QLabel *hLabell = new QLabel("h1");
    QLabel *hLabel2 = new QLabel("h2");
    QLabel *hLabel3 = new QLabel("h3");
    hLayout1->addWidget(hLabell);
    hLayout1->addWidget(hLabel2);
    hLayout1->addWidget(hLabel3);

    QLabel *vLabell = new QLabel("v1");
    QLabel *vLabel2 = new QLabel("v2");
    QLabel *vLabel3 = new QLabel("v3");
    
    QHBoxLayout* hLayout2 = new QHBoxLayout; // 水平排版二號
    hLayout2->addWidget(vLabell);
    hLayout2->addWidget(vLabel2);
    hLayout2->addWidget(vLabel3);

    // 把水平排版放進垂直排版裡
    vLayout->addLayout(hLayout1);
    vLayout->addLayout(hLayout2);

    w.show();
    return a.exec();
}
```
而在 QT5 之後有 qtcreator 可以直接用圖形化介面去做排版，非常方便。

## 3. qtcreator
```export PATH=~/Qt/Tools/QtCreator/bin/qtcreator:$PATH``` 後，或將該命令加入 .bashrc 中的最後就可以直接 ./qtcreator。新建一個專案後就會自動產生許多檔案，其中 .ui 檔就是可以配合 qtDesign 使用，直接拖拉物件來做排版。qtcreator 類似 vscode，也可以在裡面寫函數，左下角有個綠色箭頭，按下去就會自動編譯並執行。
![img](https://github.com/JrPhy/QT6/blob/main/img/QT1.jpg)
![img](https://github.com/JrPhy/QT6/blob/main/img/QT2.jpg)
