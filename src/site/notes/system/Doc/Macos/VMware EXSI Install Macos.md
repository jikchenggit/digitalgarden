---
{"author":"aming","email":"jikcheng@163.com","title":"VMware EXSI Install Macos","creation_date":"2022-10-07 12:02","Last modified date":"2022-11-25 16:11","tags":"VMware EXSI Install Macos","File Folder with relative path":"system/Doc/Macos","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/macos/v-mware-exsi-install-macos/","dgPassFrontmatter":true}
---


##  解锁vmware 
### 下载解锁补丁

[esxi-unlocker-301.tgz (github.com)](https://github.com/hugepants/esxi-unlocker/releases/download/3.0.1/esxi-unlocker-301.tgz)
### 安装补丁
```bash
./esxi-install.sh
```
### 重启ESXI
```bash
reboot
```
### 验证是否破解成功
```bash
cd /vmfs/volumes/product_HDD/exsi-unlocker

[root@localhost:/vmfs/volumes/61dff6f5-cbc78b6b-6f33-00e08b680540/exsi-unlocker] ./esxi-smctest.sh 
/bin/vmx
smcPresent = true
custom.vgz     false   39106488 B
```

> [!warning]
> 
> smcPresent = true 则表明开启成功。

## 安装过程
1. 下载ISO
2. 创建虚拟机
3. 指定ISO 
4. 启动
5. 划分磁盘
6. 开始安装。

## 配置过程
1. 安装vmware tools

3. 调整分辨率
```python
sudo /Library/Application\\ Support/VMware\\ Tools/vmware-resolutionSet  1920 1080
```

[^1] 参考《VMware虚拟机更改Mac系统的屏幕分辨率》
> [!warning]
> 目前优化很烂，根本无法使用，大家考虑清楚在安装。
> 

[^1]: [[VMware虚拟机]更改Mac系统的屏幕分辨率 - 简书 (jianshu.com)](https://www.jianshu.com/p/c19e120347c4)