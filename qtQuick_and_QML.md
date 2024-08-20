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
使用 setContextProperty 將資料傳到 QML 中，第一個引數就對應到 qml 中的物件名稱，第二個引數則是該物件的內容。setSource 則是該 .qml 檔案位置
```js
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
設定好之後就可以把該變數及內容傳到 qml 上顯示了。

