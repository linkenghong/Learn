## 1. 主机规划与磁盘分区

### 1.1 各硬件设备在Linux中的文件名

设备 | 设备在Linux内的文件名
---- | -------------------
USB或硬盘 | /dev/sd[a-p]
VirtI/O界面 | /dev/vd[a-p] （用于虚拟机内）
软盘 | /dev/fd[0-7]
打印机 | /dev/lp[0-2] 或 /dev/usb/lp[0-15]（USB 接口）
鼠标| /dev/input/mouse[0-15] （通用） /dev/psaux （PS/2界面）/dev/mouse （当前鼠标）


### 1.2 磁盘分区





