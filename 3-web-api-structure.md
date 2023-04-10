
# สร้างโครงของ Web API และทดสอบ

1. เปิดไฟล์ **Program.vb**
2. แก้ไขโค้ดเดิมให้เป็นดังด้านล่าง
    
    ```vbnet
    Imports System
    
    ' Import package สำหรับใช้ในการรันระบบ Web 
    Imports Microsoft.AspNetCore.Builder
    
    Module Program
        Sub Main(args As String())
            Console.WriteLine("Hello World!")
    				
    				' สร้าง Builder
            Dim builder = WebApplication.CreateBuilder(args)
    				
    				' สร้าง App instance
            Dim app = builder.Build()
            
            ' สร้าง HTTP Get route โดยตัว function จะคืนค่าเป็นข้อความ String กลับไปให้ client 
            app.MapGet("/", Function() "Hello World!")
    
            ' รัน Web Application instance
            app.Run()
            Console.WriteLine("Starting server!")
        End Sub
    End Module
    ```
    
3. กด F5 หรือ Start Debugging จะมี Web Browser เปิดขึ้นมาและเห็นคำว่า Hello World บนหน้าเว็บ

## หากติดปัญหาเรื่อง https

แก้ตามขั้นตอนนี้ [https://nextflow.in.th/2023/dotnet-web-api-debug-on-http-only/](https://nextflow.in.th/2023/dotnet-web-api-debug-on-http-only/)