## 个人版本-说明
相比主线版本的变更  
1. 可编辑模式安装下无需build到api文件夹  
2. 修复close时崩溃bug；vnpy_ctp 在 调用close函数登出关闭接口，大概率程序崩溃的bug，原因为C++端缺少空指针检查。  
3. 允许多次重复登录；在python的close函数里增加清理，和C++端exit函数增加重置。此时登录另一个号时可自动注销和重置接口，登录失败时也可以直接重置。  
4. 增加登录超时检查；ctp在服务器关机时连接会没有任何反应，我增加了一个登录超时检查，限定时间内没有登录成功，会重置接口并输出信息，可以设为0关闭超时检查。  
5. 延迟设定临时目录；mdapi和tdapi在 模块初始化时就设定了 临时目录路径，有点太早了，延迟到connect调用时，可以更灵活一些。

## 个人版本-分支备注
main 分支跟踪主线版本，与主线版本一致  
my 分支是个人修改版本  

----------------------------------------------------------------------------------------

# VeighNa框架的CTP期权（ETF）交易接口

<p align="center">
  <img src ="https://vnpy.oss-cn-shanghai.aliyuncs.com/vnpy-logo.png"/>
</p>

<p align="center">
    <img src ="https://img.shields.io/badge/version-3.7.1.2-blueviolet.svg"/>
    <img src ="https://img.shields.io/badge/platform-windows|linux-yellow.svg"/>
    <img src ="https://img.shields.io/badge/python-3.10|3.11|3.12|3.13-blue.svg" />
    <img src ="https://img.shields.io/github/license/vnpy/vnpy.svg?color=orange"/>
</p>

## 说明

基于CTP期权版的3.7.0接口封装开发，接口中自带的是【穿透式实盘环境】的dll文件。

## 安装

安装环境推荐基于4.0.0版本以上的【[**VeighNa Studio**](https://www.vnpy.com)】。

直接使用pip命令：

```
pip install vnpy_sopt
```


或者下载源代码后，解压后在cmd中运行：

```
pip install .
```

使用源代码安装时需要进行C++编译，因此在执行上述命令之前请确保已经安装了【Visual Studio（Windows）】或者【GCC（Linux）】编译器。

如果需要以**开发模式**安装到当前Python环境，可以使用下述命令：

```
pip install -e . --no-build-isolation --config-settings=build-dir=.\vnpy_sopt\api
```


## 使用

以脚本方式启动（script/run.py）：

```
from vnpy.event import EventEngine
from vnpy.trader.engine import MainEngine
from vnpy.trader.ui import MainWindow, create_qapp

from vnpy_sopt import SoptGateway


def main():
    """主入口函数"""
    qapp = create_qapp()

    event_engine = EventEngine()
    main_engine = MainEngine(event_engine)
    main_engine.add_gateway(SoptGateway)
    
    main_window = MainWindow(main_engine, event_engine)
    main_window.showMaximized()

    qapp.exec()


if __name__ == "__main__":
    main()
```
