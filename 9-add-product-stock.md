# เพิ่ม Stock ของ Product จาก product ID

# 1. สร้าง Class สำหรับรับข้อมูล  JSON ที่ส่งมาที่ function

1. เปิดไฟล์ **WebAPIVB/Controllers/ProductController.vb**
2. สร้าง Class ใหม่ ชื่อ **ProductUpdateModel** ไว้ด้านบนของ Controller ด้านล่าง 

```vbnet
Public Class ProductUpdateModel
    Public Property id As String ' ชื่อ property ตรงกับ field ใน json ที่ส่งเข้ามาที่ controller
    Public Property addStock As Integer ' ชื่อ property ตรงกับ field ใน json ที่ส่งเข้ามาที่ controller
End Class
```

# 2. เพิ่ม route สำหรับการขอข้อมูล product ผ่าน barcode

1. เปิดไฟล์ **WebAPIVB/Controllers/ProductController.vb**
2. สร้าง Function **GetProductById** และปรับปรุงโค้ดให้เป็นแบบตัวอย่างด้านล่างลงไป
    
    ```vbnet
    <HttpPatch("update-stock")>
        Public Function UpdateProductStock(<FromBody> model As ProductUpdateModel) As Boolean
    
            Dim result As Boolean = False
    
            Using connection As New SqlConnection(connectionString)
    
                Dim sql As String = "UPDATE products SET instock = instock + @ADDSTOCK WHERE id = @ID"
    
                Using command As New SqlCommand(sql, connection)
    
                    command.Parameters.AddWithValue("@ADDSTOCK", model.addStock)
                    command.Parameters.AddWithValue("@ID", model.id)
    
                    connection.Open()
    
                    If command.ExecuteNonQuery() > 0 Then
                        ' The query affected at least one row, so the update was successful
                        result = True
                    End If
    
                End Using
            End Using
    
            Return result
        End Function
    ```
    
3. ทดสอบการทำงาน ผ่านการกดปุ่ม F5 หรือ Start Debugging

> ในที่นี่เราจะใช้ข้อมูล product ID ที่ได้จาก **/api/Product/all**
> 
1. เปิดลิ้งค์ /swagger ใน Web browser (port ของ [localhost](http://localhost) จะแตกต่างไปตามแต่ละเครื่อง) และทดสอบใช้งาน Swagger