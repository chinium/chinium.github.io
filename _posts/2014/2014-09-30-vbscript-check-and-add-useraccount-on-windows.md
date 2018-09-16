---
layout: post
title: VBScript - Check & Add UserAccount on Windows
tags:
  - vbscript
---

### Using WMI Service ###
* GetObject("winmgmts:\\ ...
* Win32_UserAccount WMI Class  
  <http://msdn.microsoft.com/en-us/library/aa394507(v=vs.85).aspx>{:target="_blank"}


### Using ADSI (Active Directory Service Interfaces) ###
* <http://msdn.microsoft.com/en-us/library/aa746512(v=vs.85).aspx>{:target="_blank"}


### Example ###
```vbnet
' specify your new user account info
Const USERACCOUNT_ID = "newuser1"
Const USERACCOUNT_PW = "newpasswd!"

Const ADS_UF_DONT_EXPIRE_PASSWD = &H10000

Dim gStrMsg
Dim gStrComputerName


Call Main()


Function Main()
    On Error Resume Next

    Set wshNetwork = WScript.CreateObject("WScript.Network")
    gStrComputerName = wshNetwork.ComputerName
    WriteMsg "Computer Name: " & gStrComputerName

    nRet = Check_UserAccount()
    If nRet <> 0 Then
        WriteMsg "Check_UserAccount : nRet " & nRet
        wscript.quit
    End If

    nRet = Add_UserAccount()
    If nRet <> 0 Then
        WriteMsg "Add_UserAccount : nRet " & nRet
        wscript.quit
    End If
End Function

'
' Check if user account already exists or not
'
Function Check_UserAccount()
    WriteMsg "Check_UserAccount : Start"
    nRet = 0
    On Error Resume Next

    Set objWMIService = GetObject("winmgmts:\\" & gStrComputerName & "\root\cimv2")
    Set colAccounts = objWMIService.ExecQuery("SELECT * FROM Win32_UserAccount WHERE LocalAccount = 'True' AND Name = '" & USERACCOUNT_ID & "'")
    If Err.Number = 0 Then
        If colAccounts.Count = 0 Then
            WriteMsg "UserAccount ("& USERACCOUNT_ID &") does not exists"
        Else
            nRet = 100
            WriteMsg "UserAccount ("& USERACCOUNT_ID &") already exists"
        End If
    Else
        WriteErr "Error when executing wmi query"
        nRet = -999
    End If

    Check_UserAccount = nRet
    WriteMsg "Check_UserAccount : End"
End Function


'
' Add New UserAccount to Administrators Group
'
Function Add_UserAccount()
    WriteMsg "Add_UserAccount : Start"
    nRet = 0
    On Error Resume Next
    
    set objSystem = GetObject("WinNT://" & gStrComputerName & "")
    If Err.Number <> 0 Then
        WriteErr "Error when getting domain object : step 1"
        nRet = -200
        Add_UserAccount = nRet
        Exit Function
    End If

    set objUser = objSystem.Create("user", USERACCOUNT_ID)
    If Err.Number <> 0 Then
        WriteErr "Error when creating useraccount : step 2"
        nRet = -200
        Add_UserAccount = nRet
        Exit Function
    End If

    objUser.SetPassword chr(34) & USERACCOUNT_PW & chr(34)
    objUser.SetInfo
    If Err.Number <> 0 Then
        WriteErr "Error when setting password : step 3"
        nRet = -210
        Add_UserAccount = nRet
        Exit Function
    End If

    objUser.AccountDisabled = FALSE
    objUser.Put "PasswordExpired", 0
    oldFlags = objUser.Get("UserFlags")
    newFlags = oldFlags Or ADS_UF_DONT_EXPIRE_PASSWD
    objUser.Put "UserFlags", newFlags
    objUser.SetInfo
    If Err.Number <> 0 Then
        WriteErr "Error when setting password expiration : step 4"
        nRet = -220
        Add_UserAccount = nRet
        Exit Function
    End If

    ' add user account to administrators group
    Set objGroup = GetObject("WinNT://" & gStrComputerName & "/administrators,group")
    WriteMsg "- ADsPath : " & objUser.ADsPath
    objGroup.Add(objUser.ADsPath)
    If Err.Number <> 0 Then
        WriteErr "Error when adding user account to administrators group : step 5"
        nRet = -230
        Add_UserAccount = nRet
        Exit Function
    End If

    WriteMsg "UserAccount (" & USERACCOUNT_ID & ") has been added"

    gStrMsg = gStrMsg & vbcrlf & "Adding UserAccount OK"
    Add_UserAccount = nRet

    WriteMsg "Add_UserAccount : End"
End Function


Sub WriteMsg(strMessage)
    wscript.stdout.writeline strMessage
End Sub


Sub WriteErr(strMessage)
    wscript.stdout.writeline strMessage
    wscript.stdout.writeline Err.Number & " Src: " & Err.Source & " Desc: " &  Err.Description
    Err.Clear
End Sub
```

