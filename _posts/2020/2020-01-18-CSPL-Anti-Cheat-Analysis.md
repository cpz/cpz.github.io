---
layout: post
title:	"CSPL.ru Anti-Cheat Analysis"
date:	2020-01-18 04:59:00
categories:
    - blog
tags:
    - reversing
    - windows
    - CSPL
    - anticheat
---

Hello,

Today we'll talk about CSPL.ru Anti-Cheat named as "AAC" which means "Apofig Anti-Cheat". Apofig is previous name of this Russian League.

AAC is mostly user-mode anti-cheat but they have kernel mode module too.
Previously, when AAC was only user-mode Anti-Cheat in 2012 or around this, they managed to detect Organner via going through all files of player system and looking for 'config.cfg' and launcher of Organner. Similar to what Valve did to detect Organner. They was looking for USN Journal and searching for config.cfg and was reading first 2 lines which contained something like 'Organner.pl Immunity Duo Config File' and locating of config file to find launcher of organner near of config.

What they do?:
* Collect running processes
* Collect running modules in processes
* Collet names of windows
* Enumerating of running drivers
* Walking through prefetch and USN Journal
* Walking through system and hashing files
* Screenshots via GDI for Desktop screenshots and DirectX for In-Game screenshots.
* Protect process via Kernel Callbacks

The four main entities of Anti-Cheat are:

1: AAC.exe is launcher which connects with CSPL.ru `aac.cspl.ru:8008/login` (for some reason, they creating log about connection with site. Can be found there C:\senderr.txt)

Example of log:

~~~
17/1/2020 17:20:2 Starting AAC
17/1/2020 17:20:4 Send ok! CSPLOK. ID = 1
17/1/2020 17:20:18 Send ok! CSPLOK. ID = 1
17/1/2020 17:20:29 Send ok! CSPLOK. ID = 1
17/1/2020 17:20:44 Send ok! CSPLOK. ID = 1
17/1/2020 17:20:55 Send ok! CSPLOK. ID = 1
~~~

AAC.exe does loading driver `usbhubmswg.sys` which installs Callbacks.

`usbhubmswg.sys` using certificate with name 'Kazakevich Aleh' and if you'll google this name then you'll [end up there \[0\]][0]. This is how they hashing files of system.

AAC.exe does injection of `aacdx.dll` and `d3dx10_43.dll` in to game.

AAC.exe collecting running processes in system, there a example structure of single process:
~~~
struct {
  std::string windows_caption;
  std::string command_line;
  std::string local_path;
  DWORD hash;
  std::uint8_t file_size;
  std::string caption;
  dlls_stuct dlls;
  DWORD hash2;
} process_struct;
~~~

AAC.exe collecting running dlls in the process, there a example structure:
~~~
struct {
  std::string local_path;
  DWORD hash;
  std::string caption;
} dlls_struct;
~~~

2: usbhubmswg.sys installing CreateProcess, LoadImage, CreateThread, ObProccess(PreCall), ObProcess(PostCall), ObThread(PreCall), ObThread(PostCall) to protect game from injection and etc.

Driver is protected via VMProtect.

Non-finished yet.

3: aacdx.dll does VEH Hook on something?

Non-finished yet.

4: Screenshot Cleaner - Proof of Concept

Non-finished yet.

5: HWID Information.

There a structure which contains current user HWID Data (Notice: Its not complete reversed yet.)
~~~
struct {
  std::string UserName;
  std::uint8_t WinVer;
  std::uint8_t OSbit;
  std::uint8_t WinVer;
  std::string VideoCard;
  std::string DiskVolInfo; /// Disk Volume Info
  std::uint8_t HDD_Type;
  std::string HDD_Name;
  std::uint8_t HDD_RevNo; /// Revision Number
  std::uint8_t HDD_SN;   /// Serial Number
} HWID_Data;
~~~

## Credits

~~~
# https://github.com/fwx
# https://github.com/getjump
~~~

[0]: http://www.cyberforum.ru/beta-testing/thread1207634.html
