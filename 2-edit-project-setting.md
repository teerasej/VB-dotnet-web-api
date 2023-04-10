# ปรับแต่งการตั้งค่าโปรเจคให้เหมาะกับการทำ Web API

## 1. ปรับ SDK ของ Project File ให้เป็นประเภท Web

1. จาก Solution Explorer คลิกขวาที่ไฟล์โปรเจค **ProductAPI**
2. เลือกคำสั่ง Edit Project
    
    ![2023-04-09_17-26-29.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cfa998fe-6276-4d75-b207-c7ba449ff1ae/2023-04-09_17-26-29.png)
    
3. ทำการแทนที่ไฟล์เดิมด้วยโค้ดต่อไปนี้ 
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">
    
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <RootNamespace>ProductAPI</RootNamespace>
        <TargetFramework>net7.0</TargetFramework>
      </PropertyGroup>
    
    	<ItemGroup>
    		<PackageReference Include="Microsoft.AspNetCore.OpenApi" Version="7.0.0" />
    		<PackageReference Include="Swashbuckle.AspNetCore" Version="6.4.0" />
    	</ItemGroup>
    
    </Project>
    ```
    
4. จากนั้นบันทึก และปิดไฟล์

# 2. สร้างไฟล์ appsettings.json

เราจะทำการสร้างไฟล์ json ขึ้นมาใช้งาน โดยตั้งชื่อว่า **appsettings.json**

1. จาก Solution Explorer คลิกขวาที่ไฟล์โปรเจค **ProductAPI**
2. เลือกคำสั่ง **Add**
3. เลือกคำสั่ง **New Item…** จากนั้นตั้งชื่อไฟล์ตามที่กำหนด

![2023-04-09_17-18-40.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2bfcbecb-f6aa-4f3f-83dc-c634c2e89c42/2023-04-09_17-18-40.png)

จากนั้นให้เพิ่มโค้ดด้านล่างลงไปในไฟล์ 

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```