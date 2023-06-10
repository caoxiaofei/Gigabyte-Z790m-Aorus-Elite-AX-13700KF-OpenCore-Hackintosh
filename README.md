# Gigabyte-Z790-Aorus-Elite-AX-13700K-OpenCore-Hackintosh

### 我的软、硬件

- Motherboard：Gigabyte Z790 Aorus Elite ax DDR5 Ver1.0
- CPU：I7 13700K （对CPU没特别要求，别的CPU只需在config.plist中更改显示名就可以）
- GPU：Gigabyte RX6600 EAGLE 8G (只要是免驱的显卡都应该适用)
- macOS：Ventura 13.4（Ventura所有版本都适用，Monterey、Big Sur没测试过）
- OpenCore ： 0.9.2
- Bios：F6a [官方地址](https://www.aorus.com/motherboards/Z790-AORUS-ELITE-AX-rev-10/Support)



# ~~警告！！！~~

 **~~目前增量更新时卡进度条，不知道怎么解决。不建议用此EFI进行更新系统。~~**



# 关于增量更新

禁用BlueToolFixup.kext 再更新即可增量更新

我是把蓝牙相关的三个kext（BlueToolFixup.kext、IntelBluetoothFirmware.kext、IntelBTPatcher.kext）都禁用增量更新成功了。




# 请生成自己的SMBIOS

使用[OCAT](https://github.com/ic005k/OCAuxiliaryTools/releases)随机生成自己的UUID等再使用。

生成的SystemSerialNumber可以检查一下能否在官网查的到，如果提示不正常则是可用的，如果查到有保修期之类的信息，说明这个号码是苹果官方售卖的硬件使用的ID，不要用，使用后有封号风险。

一般随机生成的很大概率是查不到信息，可以使用的。



# 关于制作EFI启动盘

按照官方教程[OpenCore-Install-Guide/](https://dortania.github.io/OpenCore-Install-Guide/)操作就行。

大致可分为

<img src="https://xtvj.github.io/images/黑苹果EFI制作流程.svg" alt="黑苹果EFI制作流程" style="zoom:75%;" />

#### [制作ACPI](https://dortania.github.io/Getting-Started-With-ACPI/ssdt-methods/ssdt-easy.html#running-ssdttime)

我是使用[SSDTTime](https://github.com/corpnewt/SSDTTime)工具在Windows下生成的，操作很简单，只是当初看英文文档的时候先看的手动制作，以为很难，其实用工具在Windows下选择几个选项就自动生成了。



#### 定制自己机箱的USB驱动

使用[USBToolBox](https://github.com/USBToolBox/tool/releases)在Windows下制作驱动，替换EFI里的UTBMap.kext就可以。

如果不定制也可以使用主板上的USB接口，只是机箱上的可能识别不到。



#### U盘启动盘

~~之前是下载完整的MacOS镜像，再使用[etcher](https://github.com/balena-io/etcher)刷入U盘，之后把EFI放入U盘的EFI分区中，这样启动时总是找不到MacOS的选项。原因大概是etcher把EFI分区没有标记，用分区工具将放EFI的分区改为引导分区应该就行，自己没有测试过。~~

之前制作U盘启动盘我是用官方建议的方式：[rufus-method](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/windows-install.html#rufus-method)




现在的启动U盘制作过程（macOS系统、参考了：https://post.smzdm.com/p/a785e48l/ )

1. 使用[macadmin-scripts](https://github.com/munki/macadmin-scripts)下载相应的离线安装镜像，下载后会自动换成DMG的文件。

   [scheblein/macadmin-scripts](https://github.com/scheblein/macadmin-scripts)分支修复了macos13上使用问题

2. 加载DMG镜像文件到系统

3. sudo /Volumes/下载的镜像名/Contents/Resources/createinstallmedia --volume /Volumes/新建的空白镜像名/

4. 再挂载新建的镜像文件，并挂载其EFI文件夹，把EFI复制进去

5. 使用[etcher](https://github.com/balena-io/etcher)写入U盘



或使用[gibMacOS](https://github.com/corpnewt/gibMacOS)下载PKG文件安装到系统，再用[官方的方法](https://support.apple.com/en-us/HT201372)制作U盘也很方便



# 关于安装

安装过程中**启动Apple Logo后会有一段较长时间的屏幕熄灭**，我第一次安装的时候有好几分钟的黑屏时间，目前使用有十几秒的黑屏时间，黑屏时间过后才会出现设置界面或登录界面。



# 关于EFI

EFI会长期更新

机型改为iMac pro1,1可以提高单核性能



# 关于BIOS

并不是非要按我的设置来做，你能启动并成功安装使用的话就不用动它了。

- **Secure Boot : Disabled**

- **Internal Graphics : Disabled**

- Above 4G Decoding : Enabled

- Above 4GB MMIO BIOS assignment : Enabled

- Re-Size BAR Support : Disabled

- **Intel Platform Trust Technology(PTT): Disabled**

- **Hyper-Threading : Enabled**

- **CFG Lock : Disabled**

![image info](./img/IMG_0390.avif)

![image info](./img/IMG_0391.avif)

![image info](./img/IMG_0392.avif)

![image info](./img/IMG_0393.avif)

![image info](./img/IMG_0394.avif)

![image info](./img/IMG_0395.avif)

![image info](./img/IMG_0396.avif)

![image info](./img/IMG_0397.avif)



# 关于功能

- 能正常开机使用
- 能登录AppStore下载软件
- 睡眠未测试，我都是直接关机后断电
- iCloud、Message等未测试，可能是正常的。这些功能我没用到。
- 安装系统后，推荐使用CleanNvram清一下



# 关于跑分

R23跑分与Windows下的跑分几乎相同

如果你使用单条内存，Geekbench的跑分可能会低于预期

跑分不能说明一切，实用时软件能调用满荷CPU与GPU就行。



# 推荐的软件

- [MonitorControl](https://github.com/MonitorControl/MonitorControl) ：用来调整屏幕亮度。

- [OCAT](https://github.com/ic005k/OCAuxiliaryTools/releases) ：用来修改EFI文件

- [Stats](https://github.com/exelban/stats)：状态栏的系统监测工具



# EFI文件分享

[Gigabyte-Z790-Aorus-Elite-AX-13700K-OpenCore-Hackintosh](https://github.com/xtvj/Gigabyte-Z790-Aorus-Elite-AX-13700K-OpenCore-Hackintosh)



# 更新记录

- 2023.06.10

1. 升级部分kext，删除三个无用的ACPI文件
2. 未修复任何BUG，只是更新了点驱动

- 2023.06.05

1. 在13.4以上的系统下，蓝牙不能用，可以通过在NVRAM的7C436110-AB2A-4BBB-A880-FE41995C9F82下添加两个参数来修复

   ```
   bluetoothExternalDongleFailed Data 00
   
   bluetoothInternalControllerInfo Data 0000000000000000000000000000
   ```

- 2023.06.01

1. 添加[AirportItlwm.kext](https://github.com/OpenIntelWireless/itlwm)驱动，现在WiFi可以正常使用了。

- 2023.05.20

1. 禁用EnableVectorAcceleration，以修复显卡为Rx6600或Rx6600XT时，开机过程中的随机重启问题，参考地址：https://bbs.pcbeta.com/viewthread-1967636-1-2.html 。我已经开机二十来次，除第一次重启外都没重启。

- 2023.05.09

1. 更新到OpenCore0.9.2
2. 禁用了WiFi驱动，本来启用时WiFi也不能用。
3. 添加提示USB驱动制作

- 2023.05.03 ：

1. 添加CleanNvram工具，并在启动时显示系统外的工具类选项，如需隐藏可**勾选**Misc→Boot→HideAuxiliary
2. 更新自己的Bios为F6a，并更新Bios设置截图

