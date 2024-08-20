可以將物件與信號槽連接起來後，就可以來做專案了，先來做的最簡單的登入頁面。在此我們需要一個可以輸入帳號密碼的文字框，選用```LineEdit```以及```PushButton```，當然按下去之後還要去跟資料庫作連動，在此只是作業面而已。先開個新專案然後把 ui 拉好，其中的字體可在 design 的右下角去選擇大小。
![img](https://github.com/JrPhy/QT6/blob/main/img/QT_login.jpg)
同樣的設定兩個函數來與當入和註冊連動，再輸入密碼時通常不希望直接顯示在螢幕上，所以在右邊的 line 10 就是將輸入的密碼用圓點遮蔽，就可以看到右圖輸入的效果。

當然我們也可以給個 check box，勾選後就將 mode 設為一般，為勾選就設為遮蔽，只需將函數與 check box 作連動即可。
![img](https://github.com/JrPhy/QT6/blob/main/img/QT_login.jpg)
