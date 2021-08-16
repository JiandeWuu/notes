# Ubuntu操作

## 掛載USB

1. 查找驅動器叫什麼

   您需要知道驅動器掛載的方式。要執行此操作：

   `sudo fdisk -l`

   您正在尋找的分區應該類似於：。記住它叫什麼。 `/dev/sdb1`

2. 創建一個安裝點

   在中創建一個新目錄，以便可以將驅動器安裝到文件系統上： `/media`

   `sudo mkdir /media/usb`

3. 安裝！

   `sudo mount /dev/sdb1 /media/usb`

   完成後，請開火：

   `sudo umount /media/usb`
