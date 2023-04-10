# เรียกดูข้อมูล Product ด้วย Barcode 

# เพิ่ม route สำหรับการขอข้อมูล product ผ่าน barcode

1. เปิดไฟล์ **WebAPIVB/Controllers/ProductController.vb**
2. สร้าง Function **GetProductById** และปรับปรุงโค้ดให้เป็นแบบตัวอย่างด้านล่างลงไป
    
    ```vbnet
    <HttpGet("by-barcode/{barcode}")>
        Public Function GetProductByBarcode(ByVal barcode As String) As IEnumerable(Of Product)
    
            Dim products As New List(Of Product)()
    
            Using connection As New SqlConnection(connectionString)
    
                Dim sql As String = "SELECT * FROM products WHERE barcode = @BARCODE"
    
                Using command As New SqlCommand(sql, connection)
    
                    command.Parameters.AddWithValue("@BARCODE", barcode)
    
                    connection.Open()
                    Dim reader As SqlDataReader = command.ExecuteReader()
                    While reader.Read()
                        Dim product As New Product With {
                            .Id = CInt(reader("id")),
                            .Name = CStr(reader("name")),
                            .Barcode = CStr(reader("barcode")),
                            .InStock = CInt(reader("instock"))
                        }
                        products.Add(product)
                    End While
                    reader.Close()
                End Using
            End Using
    
            Return products
    
        End Function
    ```
    
3. ทดสอบการทำงาน ผ่านการกดปุ่ม F5 หรือ Start Debugging

> ในที่นี่เราจะใช้ข้อมูล barcode ที่ได้จาก **/api/Product/all**
> 
1. เปิดลิ้งค์ /swagger ใน Web browser (port ของ [localhost](http://localhost) จะแตกต่างไปตามแต่ละเครื่อง) และทดสอบใช้งาน Swagger