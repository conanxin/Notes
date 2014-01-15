## 摘要 ##
Visualistion is handled by thr Unity3D 2.5 multiplatform game development tool, which communicates over .Net socket servers with data feeds from a 5DT Data Glove Ultra that measures figer flexures, and an iPhone based touchpad device. The techniques are directly applicable to the monitoring of mining environments, in which mining equipment is surrounded by various sensor networks.

## 1.介绍 ##

## 2.配置和操作 ##
### A.设备 ###
The data glove is connected wirelessly to a PC using the 5DT Ultra Wireless Kit.

Unity3D 2.5 multiplatform game development tool is used to display the user interfaces and 3D objects and to process the interactions transmitted form the input devices. 

### B.数据和用户界面交互 ###
The system implements two modes of operation: (a)a view mode and (b)an interactive mode. In view mode, the user is not interacting with VR objects and only watches the scenes presented by the graphics engine. In interactive mode, the main menu is displayed on both the dome screen and the iPhone's touch screen.

### C.多传感器通信协议 ###
The central element in the system is the Data Server, which acts as an information proxy routing data between sensors and clients. For simplicity and universality, the data server has been implemented in Ruby 1.8.6 using GServer Class. Sensors connected to the data server are either hard-wired or using wireless connection and, after registration, are able to transmit information by a custom XML protool. This was developed specifically for the experments and is known as the Human Systems Integration Protocol(HSIP). 

## 3.沉浸式虚拟场景 ##
### A.Virtual Mine E-book ###

### B.Surface mine teleoperated scenario ###
In this case, the landscpe of the real surface mine is reconstructed based on Geographic Information System(GIS) data and converted to a format suitable for Unity3D. The mine surface equipment is then geo-reerenced using GPS and passed to a virtual reality data server to construct a calculated virtual mine representation.

## 4.总结 ##
CSIRO has already developed VR systems to remotely monitor mining equipment(LASC Longwall Automation, amd the Nexsys risk management system).