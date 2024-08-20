QML 是一種 QT 自行定義的語言，結合了 json+js 語法，副檔名用 .qml，簡化了 UI 的開發，qtQuick 則是用來寫 qml 的工具。開新專案後選擇 Qt Quick Application 就會幫你創建好了。當然 Qt Widget 也可以做相同的事，只不過 Qt Widget 幾乎全是 C++，對開發者來說成本比較高，而 QML 相較之下就比較好開發。

## 1. MVC 架構
全名為 Model<->Controller<->View，也就是將前後端分離。以網站來說，前端就是 html+js+css，後端則是 PYTHO/PHP/JAVA，然後資料庫在獨立出來，中間用 js 來做前後端的溝通。在 QT 就是以 QML 為前端 View，C++ 為後端 Model

## 2. 與 C++ 互動

#### 1. 上下文屬性
在 main.cpp 中加入以下程式碼
```cpp
...
#include <QQmlContext>

int main(int argc, char *argv[])
{
    ...
    engine.rootContext()->setContextProperty("Data", QString("main.qml"));
    ...
}
```
使用 setContextProperty 將資料傳到 QML 中，第一個引數就對應到 qml 中的物件名稱，第二個引數則是該物件的內容。setSource 則是該 .qml 檔案位置
```js
Window {
    ...
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
}
```
設定好之後就可以把該變數及內容傳到 qml 上顯示了。
![img](https://github.com/JrPhy/QT6/blob/main/img/qml.jpg)

#### 2. 註冊類別
需要從 QObject 繼承新的子類，並使用 Q_OBJECT 和 Q_PROPERTY 宏定義，各項詳細可參考[這篇](https://www.cnblogs.com/wanghongyang/p/15233642.html)，例子可參考[官方給的](https://doc.qt.io/qt-5/qtqml-cppintegration-topic.html)。
```Q_PROPERTY(QString userName READ userName WRITE setUserName NOTIFY userNameChanged)```
```Q_PROPERTY(型別 變數 READ 變數 WRITE 函數 NOTIFY signal)```
然後在 main.cpp 中去註冊該函數，使用 qmlRegisterType 去註冊，第一個引數填入 module name，給在 .qml 中 import 用的。
```cpp
...
#include <backend.h>
...
int main() {
...
    qmlRegisterType<BackEnd>("io.qt.examples.backend", 1, 0, "BackEnd");
...
}
```
最後在 .qml 中去 import 就可以使用了。
```js
import QtQuick 2.6
import QtQuick.Controls 2.0
import io.qt.examples.backend 1.0 //module name

ApplicationWindow {
    id: root
    width: 300
    height: 480
    visible: true

    BackEnd {
        id: backend
    }

    TextField {
        text: backend.userName
        placeholderText: qsTr("User name")
        anchors.centerIn: parent

        onTextChanged: backend.userName = text
    }
}
```
在 backend.cpp 中印出訊息，可以看到隨著我們的輸入就有一直印出文字。
![img](https://github.com/JrPhy/QT6/blob/main/img/qml_class.jpg)

#### 3. 
