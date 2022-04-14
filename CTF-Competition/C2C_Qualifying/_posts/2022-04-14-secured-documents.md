---
layout: default
title:  "secured-documents"
date:   2022-04-14
---

### Secured Documents

**Description**: One of our employees accidentally encrypted a sensitive document using an internal tool, but forgot to write down the password. Can you help us find the password? Note: The challenge file contains an Office Macro, and it is suggested to open the document and run it in a Virtual Machine.

**Attachment**: company_records.docm.zip in [Google Drive](https://drive.google.com/drive/folders/1_f0PLreUcap2YW33Yfmi_Hj1cVyd9u6w?usp=sharing)

##### Solution:

This is a Microsoft Office document locked by Macro. When opening the file , choose do not enable Macro so that the file can be open in view-only mode and we can check the Macro script. The password for the file could be found in the function:

```vba
Private Sub Document_Open()
    Dim enteredPassword As String
    enteredPassword = InputBox("Enter password to unlock document contents")
    If enteredPassword = "0038417CED1B00460BD094BC1188E05D" Then
        MsgBox ("Decrypting document contents...")
        EncryptText (enteredPassword)
    Else
        MsgBox ("Incorrect password. Please try again later")
        ActiveDocument.Close
    End If
End Sub

Public Sub EncryptText(ByVal pw As String)
    Dim docText As String
    docText = ActiveDocument.Content.Text
    
    docText = Replace(docText, "{{ ", "")
    docText = Replace(docText, " }}", "")
    
    Dim docTextBytes() As Byte
    docTextBytes = StrConv(docText, vbFromUnicode)
    Dim passwordBytes() As Byte
    passwordBytes = StrConv(pw, vbFromUnicode)
    Dim pwCounter As Integer
    pwCounter = LBound(passwordBytes)
    
    For i = LBound(docTextBytes) To UBound(docTextBytes)
        If docTextBytes(i) <> 10 Then
            pwCounter = (i Mod UBound(passwordBytes))
            docTextBytes(i) = docTextBytes(i) Xor passwordBytes(pwCounter)
        End If
    Next i
    
    docText = StrConv(docTextBytes(), vbUnicode)
    ActiveDocument.Content.Text = docText
End Sub
```


