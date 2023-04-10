# กำหนดรูปแบบการทำงานของ Web API ผ่าน Controller

# 1. สร้าง Controller สำหรับจัดการข้อมูล Product

1. สร้างโฟลเดอร์ Controllers ในโปรเจค
2. สร้างไฟล์ ProductController.vb ในโฟลเดอร์ดังกล่าว
3. เขียนไฟล์กำหนดการทำงานของ Controller ดังนี้ 
    
    ```vbnet
    Imports Microsoft.AspNetCore.Mvc
    Imports Microsoft.Extensions.Logging
    
    ' กำหนด attribute เป็น ApiController และกำหนด Route 'api/Product' โดยชื่อ Url จะเอามาจากชื่อของ Class ที่ตัดคำว่า Controller ออก
    <ApiController>
    <Route("api/[controller]")>
    Public Class ProductController
        Inherits ControllerBase
        
        ' Constructor ที่รับ logger เข้ามาใช้งานถ้าต้องการ
        Public Sub New(ByVal logger As ILogger(Of ProductController))
    
        End Sub
    
    		' สร้าง function ที่จะทำงานเมื่อได้รับ HTTP GET /api/Product/all
    		<HttpGet("all")>
        Public Function GetAllProducts() As String
            Return "all products"
        End Function
    
    End Class
    ```
    

# 2. โหลด Controller เข้าตัว builder และ Web API

1. เปิดไฟล์ **WebAPIVB/Program.vb**
2. เพิ่มโค้ดลงไปดังตัวอย่าง 
    
    ```vbnet
    Imports System
    Imports Microsoft.AspNetCore.Builder
    
    Module Program
        Sub Main(args As String())
    
            Dim builder = WebApplication.CreateBuilder(args)
    
    				' ให้ builder ทำการโหลด controller ทุกตัวที่อยู่ในโฟลเดอร์ controllers
            builder.Services.AddControllers()
    
            Dim app = builder.Build()
            app.MapGet("/", Function() "Hello World!")
    
            ' ให้แอพทำการใช้ attribute Route ของ Controller
            app.MapControllers()
    
            app.Run()
            Console.WriteLine("Starting server!")
        End Sub
    End Module
    ```
    
3. ทดสอบการทำงาน ผ่านการกดปุ่ม F5 หรือ Start Debugging
4. เปิดลิ้งค์ /api/Product/all ใน Web browser (port ของ [localhost](http://localhost) จะแตกต่างไปตามแต่ละเครื่อง) เพื่อให้เห็นข้อความที่ตอบกลับมาจาก controller