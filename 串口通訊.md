在開發時如果遇到找不到 QtSerialPort，可以嘗試做以下事情
1. sudo apt-get install libqt5serialport5-dev
2. 在 qtcreator->QT Maintain Tool 中安裝 QtSerialPort
3. cmake find_package 與 target_link_libraries 加上 SerialPort

