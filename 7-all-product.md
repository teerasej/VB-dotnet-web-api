# เรียกดูข้อมูล Product ทั้งหมดเป็น JSON

# 1. ติดตั้ง SqlClient package

1. จาก Solution Explorer เปิดส่วน Project > Dependency
2. คลิกขวาที่ Packages และเลือก Manage NuGet Packages
    
    ![2023-04-09_20-03-24.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3b2dab31-1cc1-418e-8207-6300dc363e54/2023-04-09_20-03-24.png)
    
3. จาก tab Browse > ค้นหา **SqlClient**
4. เลือก Microsoft.Data.SqlClient
5. กด Install
    
    ![2023-04-09_20-03-55.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0453a44c-a365-427a-851e-419d26bca93a/2023-04-09_20-03-55.png)
    
6. กด OK และ Accept เพื่อยอมรับเงื่อนไขการใช้งาน
    
    ![2023-04-09_20-09-11.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5e203f50-b573-40b3-9c50-f49d7207c8a0/2023-04-09_20-09-11.png)
    

# 2. สร้าง Class Product เพื่อเก็บข้อมูลใช้งานจาก Database

1. เปิดไฟล์ **WebAPIVB/Controllers/ProductController.vb**
2. เพิ่มโค้ดด้านล่างลงไป
    
    ```vbnet
    Imports Microsoft.AspNetCore.Mvc
    Imports Microsoft.Extensions.Logging
    
    ' สร้าง class Product เพื่อเป็นตัวแทนแต่ละ row ใน database table
    ' Property แต่ละตัว เป็นตัวแทนของ field ใน table 
    Public Class Product
        Public Property Id As Integer
        Public Property Name As String
        Public Property Barcode As String
        Public Property InStock As Integer
    End Class
    
    <ApiController>
    <Route("api/[controller]")>
    Public Class ProductController
        ...
    End Class
    ```
    
3. เพิ่มส่วนของ Connection string และส่วนที่รัน SQL กับ database 
    
    ```vbnet
    Imports Microsoft.AspNetCore.Mvc
    Imports Microsoft.Extensions.Logging
    Imports Microsoft.Data.SqlClient
    
    Public Class Product
        Public Property Id As Integer
        Public Property Name As String
        Public Property Barcode As String
        Public Property InStock As Integer
    End Class
    
    <ApiController>
    <Route("api/[controller]")>
    Public Class ProductController
        Inherits ControllerBase
        
        ' connection string สำหรับเชื่อมต่อ database
        Dim connectionString As String = "Server=tcp:mkctoolslab.database.windows.net,1433;Initial Catalog=productDB;Persist Security Info=False;User ID=myadmin;Password=P@ssw0rd;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"
    
        Public Sub New(ByVal logger As ILogger(Of ProductController))
    
        End Sub
    
        <HttpGet("all")>
        Public Function GetAllProducts() As IEnumerable(Of Product) ' กำหนด return เป็น Enum ของ Product เช่น List ของ Product
    				
    				' สร้าง List สำหรับเก็บข้อมูลสินค้า
    				Dim products As New List(Of Product)() 
    				
    				' เชื่อมต่อฐานข้อมูลโดยใช้ค่า connectionString
    		    Using connection As New SqlConnection(connectionString) 
    
    	        ' กำหนดคำสั่ง SQL เพื่อเลือกข้อมูลสินค้าทั้งหมด
    					Dim sql As String = "SELECT * FROM products" 
    					
    					' สร้าง SqlCommand เพื่อส่งคำสั่ง SQL ไปยังฐานข้อมูล
    	        Using command As New SqlCommand(sql, connection) 
    						
    						' เปิดการเชื่อมต่อกับฐานข้อมูล
                connection.Open() 
    
    						' ส่งคำสั่ง SQL ไปทำงานและรับผลลัพธ์เป็น SqlDataReader
                Dim reader As SqlDataReader = command.ExecuteReader() 
    
    						' วนลูปจนกว่าจะอ่านข้อมูลได้ทั้งหมด
                While reader.Read() 
    								
    								' สร้าง Object Product และกำหนดค่า Property ด้วยข้อมูลที่ได้อ่านมาจากฐานข้อมูล
                    Dim product As New Product With { 
                        .Id = CInt(reader("id")),
                        .Name = CStr(reader("name")),
                        .Barcode = CStr(reader("barcode")),
                        .InStock = CInt(reader("instock"))
                    }
                    products.Add(product) ' เพิ่ม Object Product เข้าไปใน List ของสินค้า
    
                End While
                reader.Close() ' ปิดการทำงานของ SqlDataReader เพื่อคืนทรัพยากร
    
    		    End Using
    		
    		End Using
    		
    		Return products ' ส่ง List กลับเป็น Response ของ API
    		
    End Function
    
    End Class
    ```
    
4. วาง Breakpoint ไว้ที่คำสั่ง return
5. ทดสอบการทำงาน ผ่านการกดปุ่ม F5 หรือ Start Debugging
6. เปิดลิ้งค์ /swagger ใน Web browser (port ของ [localhost](http://localhost) จะแตกต่างไปตามแต่ละเครื่อง) และทดสอบใช้งาน Swagger
7. สังเกตผลลัพธ์ของ product ตรง breakpoint และใน swagger

> ให้ทำการเลือก copy ข้อมูลของสินค้าไว้คนจะชิ้น เช่น id, barcode และ in-stock เพื่อใช้ในขั้นตอนถัดไป
>