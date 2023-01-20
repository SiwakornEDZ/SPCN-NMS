***

![zabbix_course](https://user-images.githubusercontent.com/87377798/211203255-d19edcb1-f6cf-42db-beda-9dfbaf63538a.png)

# การติดตั้ง การใช้งาน และการอ่านผลของเครื่องมือ Zabbix

***
## ขั้นตอนที่ 1 - Create Ct สำหรับเตรียมติดตั้ง Server ของ Zabbix
## ขั้นตอนที่ 2 - Setup และ Config Zabbix
## ขั้นตอนที่ 3 - Implement Zabbix โดนใช้ SNMP 
## ขั้นตอนที่ 5 - Monitor อุปกรณ์
## ขั้นตอนที่ 6 - การแจ้งเตือน (ผ่าน Outlook365) 
## ขั้นตอนที่ 7 - การอ่านผลเครื่องมือ

 
***

## ขั้นตอนที่ 1 - Create Ct สำหรับเตรียมติดตั้ง Server ของ Zabbix

## กำหนดชื่อ กำหนดรหัสผ่าน และSSH KEY 

![323349242_3367566253515291_5797434817281545862_n](https://user-images.githubusercontent.com/87377798/211203719-2ae05f21-bd9e-4a69-baf5-f57691d327ea.png)

## เลือก os ที่ต้องการ
 
![318014612_1378125582943631_2714223770837493793_n](https://user-images.githubusercontent.com/87377798/211204548-b63faece-9c4a-4e44-979d-07ad4cfb844b.png)

## กำหนดขนาดของ Storage ของ Server

![318339087_967194557599547_4498425463763641514_n](https://user-images.githubusercontent.com/87377798/211204592-fdeeba36-8708-4637-9da7-c7bbdeb8de33.png)

## กำหนดจำนวน cpu ของ Server

![323583180_825957568482783_2351984305087874723_n](https://user-images.githubusercontent.com/87377798/211204625-b46a95ae-0d5e-4d4b-9e9d-4203ebf1d9ed.png)

## กำหนดจำนวน RAM และ SWAP

![319030589_884204209379943_1826564098299007394_n](https://user-images.githubusercontent.com/87377798/211204690-5e2e8b57-a388-4fd7-8690-aed7099ba1ae.png)

## กำหนด address ipv4 

![320148166_836777634097010_8849781503976518867_n](https://user-images.githubusercontent.com/87377798/211204720-91cba0dc-971f-4458-ae1c-9932a58b07bb.png)

## กำหนด DNS 

![321966742_675207904077387_8678904671529243397_n](https://user-images.githubusercontent.com/87377798/211204860-435ef51a-0ae8-40a1-a67a-bb0cfd25fb9f.png)

***

## ขั้นตอนที่ 2 - Setup และ Config Zabbix

***

## 1.Install Zabbix repository

```
wget https://repo.zabbix.com/zabbix/6.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.2-4%2Bubuntu22.04_all.deb
dpkg -i zabbix-release_6.2-4+ubuntu22.04_all.deb
```

![317835043_1298677780982128_2291772008469290255_n](https://user-images.githubusercontent.com/87377798/211205010-ae5a81f0-2c81-497a-a7da-a9020a93748e.png)

***

```
apt update
```

![319120873_5790704187685595_7086940751827624812_n](https://user-images.githubusercontent.com/87377798/211205109-8af716a9-2e7b-4959-baee-a0fae099756d.png)

***

## 2.Install Zabbix server, frontend, agent

```
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent
```

## Create initial database

```
mysql -uroot -p
```
```
create database zabbix character set utf8mb4 collate utf8mb4_bin;
```
```
create user zabbix@localhost identified by 'password';
```
```
grant all privileges on zabbix.* to zabbix@localhost;
```
```
set global log_bin_trust_function_creators = 1;
```
```
quit;
```

```
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
```
 ปิด log_bin_trust_function_creators option หลังจากนำข้อมูลเข้า database
 
```
mysql -uroot -p
```
## ใส่ password
```
set global log_bin_trust_function_creators = 0;
```
```
quit;
```

***

## Configure the database for Zabbix server

```
nano /etc/zabbix/zabbix_server.conf
```

## แก้ไข DBPassword=password

***

## เปิดการใช้งาน Server Zabbix

```
 systemctl restart zabbix-server zabbix-agent apache2
```
```
 systemctl enable zabbix-server zabbix-agent apache2
```

## ตั้งค่าใน Web Zabbix

## ตั้งค่าภาษา 
![322530354_844190266871842_7001748652109754554_n](https://user-images.githubusercontent.com/87377798/211206257-a7f66da7-eb50-4a67-9683-eb3cee6ab28d.png)

## เช็ค status ของ php

![318575298_1997417867135496_5747201532213256981_n](https://user-images.githubusercontent.com/87377798/211206295-14976b52-6723-43b6-a04b-9613d84483b5.png)

## ตั้งค่า database 

![319910391_723861215966851_1034635680952936012_n](https://user-images.githubusercontent.com/87377798/211206300-cb0900d8-a366-4d04-9df4-5016c56d3689.png)

## ตั้งค่า Timezone และ theme
![319824977_683672683433656_3733177406443467590_n](https://user-images.githubusercontent.com/87377798/211206351-49aeaf2a-7e22-43fa-8cbb-e667006a0714.png)

## เสร็จสิ้นการตั้งค่า

![317318760_647352243851877_7964469790553264821_n](https://user-images.githubusercontent.com/87377798/211206410-96b2bc81-0dea-4b2e-b2ca-718094fcc3bc.png)

![318945220_601296421998007_126078065550588884_n](https://user-images.githubusercontent.com/87377798/211206429-3e06b162-a66a-4d9c-a3f9-1588f5b3cd69.png)

## เวอร์ชั่นล่าสุดต้องทำการแก้ไข ติดตั้งภาษา local ในเครื่อง Server

![322822677_1296825954213185_4717355358310065091_n](https://user-images.githubusercontent.com/87377798/211206440-ed124418-7759-4935-a1b8-33a3d1e02a96.png)

![318581762_888040522548276_4411690178153015576_n](https://user-images.githubusercontent.com/87377798/211206478-11ea2e34-46b7-41bc-9819-dc8e57756753.png)

## แก้ไขเสร็จสิ้น

![319326173_975429466907173_3869390881423672163_n](https://user-images.githubusercontent.com/87377798/211206493-7f1efc69-263c-4919-8c3e-726efdab92ca.png)

***

## 3.Implement Zabbix โดนใช้ SNMP

```
apt-get update
```
```
apt-get install snmpd snmp
```
```
nano /etc/snmp/snmpd.conf
```
## เพิ่มและแก้ไขคำสั่งในไฟล์ snmpd.conf

```
agentAddress udp:กำหนดip:161 
view systemonly included .1.3.6.1.2.1.1
view systemonly included .1.3.6.1.2.1.25.1
view systemonly included .1.3
rocommunity public default -V systemonly
rocommunity6 public default -V systemonly
rouser authOnlyUser
sysLocation Sitting on the Dock of the Bay
sysContact Me <me@example.org>
sysServices 72
proc mountd
proc ntalkd 4
proc sendmail 10 1
disk / 10000
disk /var 5%
includeAllDisks 10%
load 12 10 5
trapsink localhost public
iquerySecName internalUser
rouser internalUser
defaultMonitors yes
linkUpDownNotifications yes
extend test1 /bin/echo Hello, world!
extend-sh test2 echo Hello, world! ; echo Hi there ; exit 35
master agentx
```
## แก้ไขเฉพาะ 3 ตัวหลัก
rocommunity ชื่อ community
syslocation ตั้งชื่อ
sysContact ชื่ออีเมล <email>;

เมื่อแก้ไขเสร็จสิ้นให้รันคำสั่ง
 
```
service snmpd stop
```
```
service snmpd start
```
```
service snmpd restart
```

## และรันคำสั่ง
 
```
snmpwalk -v2c -c ชื่อCommunity ipเครื่อง
```
***

## กำหนดชื่อ
## กำหนด ip
## กำหนด template
## กำหนด Host group
## กำหนด tool 
 
![image](https://user-images.githubusercontent.com/87377798/211206944-b499d552-6f43-4b35-b2ae-5569bbf10762.png)
 
 ## กำหนด macro
 
![image](https://user-images.githubusercontent.com/87377798/211207021-67f29eef-76ed-4e8b-b6ad-0f4d3f758e11.png)

***
 
เสร็จสิ้นการทำ SNMP

![Screenshot 2023-01-08 231127](https://user-images.githubusercontent.com/87377798/211207101-2101acdb-fae4-42a0-ab2f-68293abfd0f8.png)

***

# 4.**Monitor** อุปกรณ์   
 **ขั้นตอน**   
1.เลือก **Map**

<div align="center">
  <img src="https://user-images.githubusercontent.com/119165533/211201693-0313012f-dc35-49c5-90d1-8a4fa90c2011.png" width="700">
</div>

2.กดเลือก **All map** และเลือก **Local network**  
   - สามารถกำหนดขนาดได้ที่ **properties**  
 
<div align="center">
  <img src="https://user-images.githubusercontent.com/119165533/211201894-04e9752e-7027-45f0-8583-455fb85791f6.png" width="700">
</div>

3.กดเลือก **Edit map** 

<div align="center">
  <img src="https://user-images.githubusercontent.com/119165533/211201929-a1bf1f89-3328-497a-b4f3-fdb3df3b5a29.png" width="700">
</div>

4.กด **add** เครื่องที่เราต้องการ  

<div align="center">
  <img src="https://user-images.githubusercontent.com/110905426/213644174-94cd2543-b54e-4e56-b53d-678a66999bb7.png" width="700">
</div>

5.ตั้งค่า **Map element** และ **Mass update elements**  
- **Map element**  
    
<div align="center">
   <img src="https://user-images.githubusercontent.com/110905426/213644566-b227d301-e8ef-4136-aa14-65be370d462b.png" width="700">
</div>
<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211202076-3723e66a-0be1-43d7-af66-1f85dff904c1.png" width="700">
</div>
 
- **Mass update elements** 

<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211202098-31ecf739-361d-4bcb-9fd7-dffde5295229.png" width="700">
</div>

6.**Monitor** อุปกรณ์ทุกตัวเรียบร้อย กด **Update**  
   - ถ้าเมื่อไหร่ที่เครื่อง **server** เรามีปัญหา จะขึ้นอักษรสีแดงหรือสีอื่นก็ได้ตามที่เรากำหนดไว้  
   
<div align="center">
   <img src="https://user-images.githubusercontent.com/110905426/213643739-f43170dd-101c-43e0-b5e1-153432e36bd5.png" width="700">
</div>
<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211203691-3ae1fac3-de36-411b-bc47-778fc190bfe9.jpg" width="700">
</div>

7.ปัญหาที่เราอยากให้แจ้งเตือนเมื่อพบ สามารถเลือกได้หลากหลาย  
- **7.1 กด Mass update elements>Links Edit**  
   
<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211202376-ebc35033-ffb1-4e22-84e0-5fca8fff1bb7.png" width="700">
</div>


- **7.2 Add**> เลือก **Host**>เลือกข้อมูลที่ต้องการแสดงผล  
   
<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211202444-4328c950-2cd6-4159-b74f-5b9a7fe0521b.png" width="700">
</div>
<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211202451-b7f43991-9319-4ec6-9d3e-879a6cd4aae9.png" width="700">
</div>

8.สามารถเพื่ม **Map** อุปกรณ์บนหน้า **Dashboard**ได้  
- **8.1** เลือก**Edit Dashboard** ที่หน้า **Mornitoring zabbix** 

<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211202805-65367ba3-d5ee-4457-a6c1-3540a9e62b0e.png" width="700">
</div>

- **8.2 กด Add widget** เลือกเครื่องมือที่เราอยากเพิ่มลง  

<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211202799-92823d5c-3ee1-4c06-a083-c55573b66ba7.png" width="700">
</div>
  
# 5.**การแจ้งเตือน (ผ่าน Outlook365)**  
1.กดเลือก **User setting**  

<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211202928-aa3105c5-4e1c-4f30-b2b1-fd2ff3d2bd78.png" width="700">
</div>
 
2.ตั้งค่า **User,Media,Messaging**  

<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211203010-1789c3ce-bfce-4cd4-9035-b58d6cf77e4b.png" width="700">
</div>

- **Add** บัญชี (บัญชี Outlook365)  

<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211203073-dd39f9b2-e0b2-46cf-bc77-fc24e5fbf5e0.png" width="700">
</div>

<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211203079-629827df-6395-454d-b60f-14cf390aa8d3.png" width="700">
</div>

3.กด **Administration>Media Types**  

<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211203114-99e7c419-30e2-4c70-9e55-08c35d926457.png" width="700">
</div>

- **3.1**ตั้งค่า **Media type>Message Templates>Option**  

<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211203216-cbc66bd6-0468-44cf-bec3-7259d14ce138.png" width="700">
</div>
<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211203222-8e464400-ebed-4539-bf7a-70eca0549731.png" width="700">
</div>
<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211203234-8750180f-ebb5-4862-923d-c4e95ea3f93a.png" width="700">
</div>

4.การตั้งค่าทุกอย่างเสร็จเรียบร้อยจะขึ้นหน้าต่างแบบนี้ พร้อมกับเมลที่เรากำหนดลงไป  

<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211203265-6952fc51-194a-4294-81ae-9ee37c503ec6.png" width="700">
</div>

- **4.1** ทำการ **test** ว่าสามารถส่งแจ้งเตือนได้จริงไหม  
- กด **Test**>ใส่เมลที่เราต้องการ **Test**  

<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211203310-babda0bd-5f7f-4419-bcf9-fcc8ca551218.png" width="700">
</div>

- ถ้าสำเร็จจะขึ้นข้อความ **Media type test successful**  

<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211203418-6c6ec4da-36d4-4782-9ca9-faa1d72ad303.png" width="700">
</div>

- แจ้งเตือนที่ได้รับบน **Outlook365**  

<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211203497-d3329a1c-5351-4aa9-8f84-ef889ceb689a.png" width="700">
</div>

5.ถ้า**Zabbix** ตรวจพบเหตุการณ์ที่คาดว่าจะเป็นปัญหา จะทำการส่งแจ้งเตือน  
แจ้งเตื่อน3รูปแบบ  
•	แจ้งเตือนด้วยเสียงบน **Zabbix**  
•	แจ้งเตือนเป็นข้อความบน **Zabbix**  
•	แจ้งเตือนทาง **Outlook365** 

<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211203590-5293298f-1828-4ad8-9c96-c9f8a85f5628.png" width="700">
</div>
<div align="center">
   <img src="https://user-images.githubusercontent.com/119165533/211203598-fda342a7-2aab-4a45-95df-01c0a16ee9d2.png" width="700">
</div>

***

## จัดทำโดย 

### นายศิวกร กาญธนะบัตร        116310462002-1      scpn021 
### นางสาวฉัตรพร แก้วเฉลิม       116310400245-1      spcn07 
### นายรพีพัฒน์ พรมฮวด	        116310400423-4      spcn14 
### นายปิยะ หลามสกุล           116310462023-7      spcn27 

***







  






















