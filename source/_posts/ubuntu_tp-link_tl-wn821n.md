title: 'Ubuntu下安装TP Link TL-WN821N 无线网卡驱动'  
tags:  
  - tl-wn821n  
  - ubuntu  
  - 无线网卡  
date: 2011-01-19 14:05:07  
---  

前几天笔记本自带的无线网卡似乎坏了，系统没有识别这个设备，就买了TP Link的USB无线网卡TL-WN821N。插上后发现Ubuntu根本没有识别这个设备，还是无法上网。  

郁闷之下，切回windows（TP Link随机光盘带有windows的驱动），上网搜索后发现有[madwifi](http://madwifi-project.org/)这个开源项目，但是该项目目前还不支持usb接口的网卡（其他两个版本ath5k/ath9k的设备支持列表也没找到TL-WN821N）。后来发现有人提到用ndiswrapper可以安装windows下的驱动文件，并且用IOGEAR GWU623.zip里面的驱动文件可以正常安装使用。但我这台机安装ndiswrapper后再装上GWU623的驱动，却还是无法上网！删掉这个驱动后用TL-WN821N自带光盘里的驱动文件，能正常上网了，oh yeah
