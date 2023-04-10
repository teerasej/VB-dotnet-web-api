# เปิดการทำงานของ Swagger

![2023-04-09_19-58-05.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1c95ad40-d1cd-441b-921f-341f5e17fe0f/2023-04-09_19-58-05.png)

1. เปิดไฟล์ **WebAPIVB/Program.vb**
2. เพิ่มโค้ดลงไปดังตัวอย่าง 
    
    ```vbnet
    Imports System
    Imports Microsoft.AspNetCore.Builder
    
    ' เพิ่ม imports
    Imports Microsoft.Extensions.DependencyInjection
    
    Module Program
        Sub Main(args As String())
    
            Dim builder = WebApplication.CreateBuilder(args)
            builder.Services.AddControllers()
    				
    				' เพิ่มการทำงานของ Swagger
            builder.Services.AddEndpointsApiExplorer()
            builder.Services.AddSwaggerGen()
    
            Dim app = builder.Build()
    				
    				' กำหนด App ให้เรียก Swagger ขึ้นมาใช้งาน
            app.UseSwagger()
            app.UseSwaggerUI()
    
            app.MapGet("/", Function() "Hello World!")
    
            app.MapControllers()
    
            app.Run()
            Console.WriteLine("Starting server!")
        End Sub
    End Module
    ```
    
3. ทดสอบการทำงาน ผ่านการกดปุ่ม F5 หรือ Start Debugging
4. เปิดลิ้งค์ /swagger ใน Web browser (port ของ [localhost](http://localhost) จะแตกต่างไปตามแต่ละเครื่อง) และทดสอบใช้งาน Swagger