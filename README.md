# HelloAssembly

The smallest possible complete Windows application.

From the episode ["Hello, Assembly! Retrocoding the World's Smallest Windows App in x86 ASM"](https://youtu.be/b0zxIfJJLAY) on Dave's Garage:  
[![Hello, Assembly! Retrocoding the World's Smallest Windows App in x86 ASM](http://i3.ytimg.com/vi/b0zxIfJJLAY/hqdefault.jpg)](https://youtu.be/b0zxIfJJLAY)

And the follow-up episode ["C vs ASM: Making the World's SMALLEST Windows App"](https://youtu.be/-Vw-ONPfaFk):  
[![C vs ASM: Making the World's SMALLEST Windows App](http://i3.ytimg.com/vi/-Vw-ONPfaFk/hqdefault.jpg)](https://youtu.be/-Vw-ONPfaFk)

## Introduction

Code in the repository:

- Current code is in the folder "Lasse"
- Original code in the folder "TinyOriginal"
- QRCode is dead code, was the exe embedded into a QRCode, kept just for posterity

The goal of this project is to make the smallest possible application, without compression, that has the following features:

- Runs a Windows message loop
- Has a title bar, minimize, maximize, and close buttons, which all work as expected
- Has a system menu with the same
- Paints the background and some text centered in the middle, equal to or larger than "Dave's Tiny App"
  
Please keep it readable and explain what you're doing in the comments!  And the smaller, the better!  

## Note on virus scanners

The optimizations embedded in the current code in the Lasse directory, and those applied by Crinkler in general (if used) are rather aggressive, and seem to resemble strategies applied by certain types of malware. Because of this, your virus scanner may flag the executables you build out of the source code as "suspected malware". That can make it impossible to start them, or your virus scanner can even put them in quarantine. Depending on the antivirus solution you use, you may be able to whitelist/exclude the executables to restore them and get them to work (this is known to happen with F-Secure SAFE). It's also possible that you have to temporarily disable your virus scanner altogether (which has been observed with Microsoft Defender).

<!-- markdownlint-disable-next-line MD033 -->
<ins>**Please note:**</ins>

- It is entirely your responsibility to decide if you want to disable your virus scanner or not.
- If you do temporarily disable your virus scanner, we strongly recommend you re-enable it immediately after you test the executables you build out of this repository's code.
- Although we **do** promise that this repository does not contain any malicious code, we **cannot** guarantee that this is true for any code or executables (like supposed builds of this repository's applications) that you get from other sources.

If your virus scanner intervenes when you try to run the executables that come out of this repository's source code, and you don't feel comfortable with (partly) disabling your virus scanner, then don't do it. We do understand and we really won't hold it against you. :)

## Build instructions

### Plain MASM32

The current code in the Lasse directory can be built with plain MASM32 11.0, which can be obtained from a number of sources. Build instructions using it are:

```shell
ml /coff LittleWindows.asm /link /merge:.rdata=.text /merge:.data=.text /align:4 /subsystem:windows LittleWindows.obj
```

The executable will be named LittleWindows.exe.

### MASM32 with Crinkler

Crinkler is a compressing linker for Windows, specifically targeted towards executables with a size of just a few kilobytes. A copy of the tool is included in this repository in the Crinkler directory. It can also be acquired from [its GitHub repository](https://github.com/runestubbe/Crinkler).

Crinkler requires the Windows SDK to be installed. Best (i.e. smallest) results have been achieved with version 10.0.20348.0 of the Windows 10 SDK. It, and other versions can be downloaded from the [Windows SDK archive page](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive/) on the Microsoft website.

After installing it, the build instructions for both applications are:

- Current code in the Lasse directory:

  ```shell
  ml /c /coff LittleWindows.asm
  crinkler.exe /NODEFAULTLIB /ENTRY:start /SUBSYSTEM:WINDOWS /TINYHEADER /NOINITIALIZERS /UNSAFEIMPORT /ORDERTRIES:1000 /TINYIMPORT /LIBPATH:"C:\Program Files (x86)\Windows Kits\10\Lib\10.0.20348.0\um\x86" kernel32.lib LittleWindows.obj
  ```

  The executable will be named out.exe.

- Original code in the TinyOriginal directory:

  ```shell
  ml /c /coff /IC:\masm32\include Tiny.asm 
  crinkler.exe /ENTRY:MainEntry /SUBSYSTEM:WINDOWS /TINYHEADER /NOINITIALIZERS /UNSAFEIMPORT /ORDERTRIES:2000 /TINYIMPORT /LIBPATH:"C:\Program Files (x86)\Windows Kits\10\Lib\10.0.20348.0\um\x86" kernel32.lib user32.lib gdi32.lib Tiny.obj /OUT:tiny.exe
  ```

  The executable will be named tiny.exe.

## Current sizes

Current smallest known working executable sizes as of 01/11/2023 are as follows:

| Program | Linker | Size in bytes |
|-|-|-:|
| `Lasse\LittleWindows.asm` | MASM32 | 1104 |
| `Lasse\LittleWindows.asm` | Crinkler | 818 |
| `TinyOriginal\Tiny.asm` | Crinkler | 596 |
