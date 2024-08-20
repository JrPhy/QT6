QML 是一種 QT 自行定義的語言，結合了 json+js 語法，副檔名用 .qml，簡化了 UI 的開發，qtQuick 則是用來寫 qml 的工具。開新專案後選擇 Qt Quick Application 就會幫你創建好了。當然 Qt Widget 也可以做相同的事，只不過 Qt Widget 幾乎全是 C++，對開發者來說成本比較高，而 QML 相較之下就比較好開發。

## 1. MVC 架構
全名為 Model<->Controller<->View，也就是將前後端分離。以網站來說，前端就是 html+js+css，後端則是 PYTHO/PHP/JAVA，然後資料庫在獨立出來，中間用 js 來做前後端的溝通。在 QT 就是以 QML 為前端 View，C++ 為後端 Model

## 2. 與 C++ 互動

#### 1. 上下文屬性
在 main.cpp 中加入以下程式碼
```cpp
#include <QGuiApplication>
#include <QQmlApplicationEngine>
#include <QQuickView>
#include <QQmlContext>

int main(int argc, char *argv[])
{
    QCoreApplication::setAttribute(Qt::AA_EnableHighDpiScaling);

    QGuiApplication app(argc, argv);

    QQuickView view;
    view.rootContext()->setContextProperty("Data", QString("main.qml"));
    view.setSource(QUrl("../../main.qml"));
    view.show();

    return app.exec();
}

```
```
import QtQuick 2.9
import QtQuick.Window 2.2

Rectangle {
    width: 640
    height: 480
    color: "lightgray"

    Text {
        id: text
        anchors.centerIn: parent
        text: Data
    }
}
```
2. 
QML 暴露对象给 C++ 进行交互；
C++ 创建 QML 对象并进行交互；
C++ 对象与 QML 通过信号槽交互。
