# 安裝參考
https://web.stanford.edu/dept/cs_edu/resources/qt/install-linux

# QTCreator
export PATH=~/Qt/Tools/QtCreator/bin/:$PATH

# Cmake 錯誤
在裡面加入 set(CMAKE_PREFIX_PATH "/home/{username}/Qt/{version}/gcc_64/lib/cmake")

# 如何跨平台
與 JAVA 不同，C++ 幾乎在各平台有一樣的寫法，但會有些許差異，QT 就是把這些差異拿出來，通過 QT 的 API 去生成對應的部分，再由各平台的編譯器編成執行檔，所以必須安裝不同的編譯器，但是可以用 CMAKE 來做到 cross-compile
