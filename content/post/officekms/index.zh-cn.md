---
title: OfficeKMS软件
description: OfficeKMS激活 学习用
date: 2023-04-14T18:33:41+07:00
image: 
math: false
license: false
hidden: false
comments: true
draft: false
slug: office-kms
categories:
          - windows电脑软件
tags:
    - officeKMS
    - KMS激活
    - office激活器
lastmod: 2023-04-14T18:33:41+07:00
keywords: "office,kms,激活office,office"
---

# Office KMS激活器

>  不允许用来做违法的事情，请体验完毕之后24H内删除  

## 可以直接用的EXE文件

由于原本的CMD指令太不方便了，所以我重新封装了一套EXE版本的，EXE的规格是x32估计很多电脑都可以运行。注意！本软件仅支持激活2010版本和2013版本2016版本

> [蓝奏云](https://pvphack.lanzoue.com/iTNcy0t0kp4j) 访问密码：`hceu`
> 
> 为了防止破解我加了壳子OVO

## 源代码

```batch
@ECHO OFF
REM  QBFC Project Options Begin
REM HasVersionInfo: Yes
REM Companyname: LEl_FENG Studio
REM Productname: Office KMS激活器
REM Filedescription: 本KMS激活器支持Office 2010,Office 2013,Office 2016
REM Copyrights: 禁止使用本程序违法
REM Trademarks: 114514
REM Originalname: Office kms
REM Comments: 禁止使用本程序违法
REM Productversion: 13.24.41.23
REM Fileversion: 11.41.25.32
REM Internalname: office kms
REM ExeType: console
REM Architecture: x86
REM Appicon: blog\blog\my blog\static\favicon.ico
REM AdministratorManifest: No
REM  QBFC Project Options End
@ECHO ON
@echo off
mode con cols=75 lines=25
title=Office LEl_FENG
setlocal EnableDelayedExpansion&color 71 & cd /d "%~dp0"
%1 %2
ver|find "5.">nul&&goto :start
mshta vbscript:createobject("shell.application").shellexecute("%~s0","goto :start","","runas",1)(window.close)&goto :eof

:start
set KMS_Sev=kms9.MSGuides.com
cls
echo 正在检查网络情况……耐心等待
echo.
ping cloud.tencent.com | find "超时"  > NUL &&  goto fail
ping cloud.tencent.com | find "目标主机"  > NUL &&  goto fail
echo 本机网络良好……
goto office

:office
echo 检查安装的office……
call :GetOfficePath 14 Office2010
call :ActOffice 14 Office2010
call :GetOfficePath 15 Office2013
call :ActOffice 15 Office2013
if exist "%ProgramFiles%\Microsoft Office\Office16\ospp.vbs" set _Office16Path=%ProgramFiles%\Microsoft Office\Office16
if exist "%ProgramFiles(x86)%\Microsoft Office\Office16\ospp.vbs" set _Office16Path=%ProgramFiles(x86)%\Microsoft Office\Office16
if DEFINED _Office16Path (echo.&echo 已发现 Office2016
    call :ActOffice 16 Office2016
  ) else (echo.&echo 未发现 Office2016)


echo.&pause
exit

:ActOffice
if DEFINED _Office%1Path (
    cd /d "!_Office%1Path!"
    if %1 EQU 16 call :Licens16
    echo.&echo 尝试激活 %2 ...&echo.
    cscript //nologo ospp.vbs /sethst:kms.cangshui.net >nul
    cscript //nologo ospp.vbs /act | find /i "successful" && (
        echo.&echo ***** %2 激活成功 ***** & echo.) || (echo.&echo ***** %2 激活失败 ***** & echo.)
)    
cd /d "%~dp0"
goto :EOF

:GetOfficePath
echo.&echo 检测 %2 系列产品的安装路径...
set _Office%1Path=
set _Reg32=HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Office\%1.0\Common\InstallRoot
set _Reg64=HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Office\%1.0\Common\InstallRoot
reg query "%_Reg32%" /v "Path" > nul 2>&1 && FOR /F "tokens=2*" %%a IN ('reg query "%_Reg32%" /v "Path"') do SET "_OfficePath1=%%b"
reg query "%_Reg64%" /v "Path" > nul 2>&1 && FOR /F "tokens=2*" %%a IN ('reg query "%_Reg64%" /v "Path"') do SET "_OfficePath2=%%b"
if DEFINED _OfficePath1 (if exist "%_OfficePath1%ospp.vbs" set _Office%1Path=!_OfficePath1!)
if DEFINED _OfficePath2 (if exist "%_OfficePath2%ospp.vbs" set _Office%1Path=!_OfficePath2!)
set _OfficePath1=
set _OfficePath2=
if DEFINED _Office%1Path (echo.&echo 已发现 %2) else (echo.&echo 未发现 %2)
goto :EOF

:Licens16
for /f %%x in ('dir /b ..\root\Licenses16\project???vl_kms*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" >nul
for /f %%x in ('dir /b ..\root\Licenses16\proplusvl_kms*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" >nul
for /f %%x in ('dir /b ..\root\Licenses16\standardvl_kms*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" >nul
for /f %%x in ('dir /b ..\root\Licenses16\visio???vl_kms*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" >nul
for /f %%x in ('dir /b ..\root\Licenses16\project???vl_mak*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" >nul
for /f %%x in ('dir /b ..\root\Licenses16\proplusvl_mak*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" >nul
for /f %%x in ('dir /b ..\root\Licenses16\standardvl_mak*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" >nul
for /f %%x in ('dir /b ..\root\Licenses16\visio???vl_mak*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" >nul
cscript ospp.vbs /inpkey:NYH39-6GMXT-T39D4-WVXY2-D69YY >nul
cscript ospp.vbs /inpkey:7WHWN-4T7MP-G96JF-G33KR-W8GF4 >nul
cscript ospp.vbs /inpkey:RBWW7-NTJD4-FFK2C-TDJ7V-4C2QP >nul
cscript ospp.vbs /inpkey:XQNVK-8JYDB-WJ9W3-YJ8YR-WFG99 >nul
cscript ospp.vbs /inpkey:YG9NW-3K39V-2T3HJ-93F3Q-G83KT >nul
cscript ospp.vbs /inpkey:PD3PC-RHNGV-FXJ29-8JK7D-RJRJK >nul
goto :EOF

exit
:fail
cls
echo 连接不了服务器……QWQ
pause
```