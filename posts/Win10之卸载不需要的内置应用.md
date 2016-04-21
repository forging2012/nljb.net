---
title: Win10之卸载不需要的内置应用
date: '2016-04-15'
description:
categories:

tags:windows

---

>

### Win10之卸载不需要的内置应用

>

**对于通过Windows商店安装的应用，可以直接在开始菜单的磁贴上右键选择“卸载”命令来移除**

**但这个方法对照片、音乐、OneNote、相机等Win10预装应用无效，因为你找不到“卸载”这个选项**

**不过微软并未真正封堵卸载的途径，用户仍可以通过PowerShell这个系统工具以命令行方式卸载预装应用**

>

---

>

***细步骤如下***

* 以管理员身份运行 Cmd 并执行 powershell
* 查看安装应用 Get-AppxPackage
* 卸载包含应用 Get-Appxpackage *onenote* | Remove-AppxPackage

>

---

>

      // 日历、邮件
      get-appxpackage *communicationsapps* | remove-appxpackage

      // 人脉
      get-appxpackage *people* | remove-appxpackage

      // Groove 音乐
      get-appxpackage *zunemusic* | remove-appxpackage

      // 电影和电视
      get-appxpackage *zunevideo* | remove-appxpackage

      ·命令 get-appxpackage *zune* | remove-appxpackage 可以同时删除上两项

>

      // 财经
      get-appxpackage *bingfinance* | remove-appxpackage

      // 资讯
      get-appxpackage *bingnews* | remove-appxpackage

      // 体育
      get-appxpackage *bingsports* | remove-appxpackage

      // 天气
      get-appxpackage *bingweather* | remove-appxpackage

      ·命令 get-appxpackage *bing* | remove-appxpackage 可同时删除上述四项

>

      // OneNote
      get-appxpackage *onenote* | remove-appxpackage

      // 闹钟和时钟
      get-appxpackage *alarms* | remove-appxpackage

      // 计算器
      get-appxpackage *calculator* | remove-appxpackage

      // 相机
      get-appxpackage *camera* | remove-appxpackage

      // 照片
      get-appxpackage *photos* | remove-appxpackage

      // 地图
      get-appxpackage *maps* | remove-appxpackage

      // 语音录音机
      get-appxpackage *soundrecorder* | remove-appxpackage

      // XBox
      get-appxpackage *xbox* | remove-appxpackage

>

---

>

***恢复卸载的应用***

    Get-AppxPackage -allusers |
    foreach {Add-AppxPackage -register “$（$_。InstallLocation）appxmanifest.xml”
    -DisableDevelopmentMode}

>