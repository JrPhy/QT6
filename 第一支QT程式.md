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
然後建立 build 資料夾，並在下方建立一個 main.cpp 輸入以下程式碼
```cpp
#include <QApplication>
#include <QLabel>
#include <QWidget>
 
int main(int argc, char *argv[]) {
    QApplication app(argc, argv);
    QLabel hola("<center>Hello</center>");
    hola.setWindowTitle("My 1st QT");
    hola.resize(600, 400);
    hola.show();
    return app.exec();
}
```
最後執行```cmake ../``` 然後 ```make``` 就可以編譯出執行檔，執行後就會跳出視窗，標題為```My 1st QT```，內容為```Hello```。
