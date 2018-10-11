Private Sub copy_csvfile_to_excel()

Dim MyPath$, myFile$, AK As Workbook

   Application.ScreenUpdating = False

   MyPath = ThisWorkbook.Path & "\"

   myFile = Dir(MyPath & "*.csv")

   Do While myFile <> ""

      If myFile <> ThisWorkbook.Name Then

         Set AK = Workbooks.Open(MyPath & myFile)

         AK.Sheets(1).Copy After:=ThisWorkbook.Sheets(ThisWorkbook.Sheets.Count)

         Workbooks(myFile).Close

         End If

      myFile = Dir

      Set AK = Nothing

    Loop

   Application.ScreenUpdating = True

   ActiveWorkbook.Save

   MsgBox "汇总完成，请查看！", 64, "提示"

End Sub