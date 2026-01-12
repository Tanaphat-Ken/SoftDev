# Docker & Workshop
## About Docker
### Docker Containers
1. มีโครงสร้างแบบ Layer 3 ส่วน Application, Runtime/Libraries, OS
2. Shared OS Kernel ซึ่งเป็นจุดเด่นที่ทำให้ Container ต่างจาก VM คือ การ Allocate Resource ที่ไม่ต้องทำตลอด
3. OS ที่รองรับมีเกือบทุกเจ้าทั้ง Linux, Windows, MacOS

### Containers vs VM
1. Container ทำงานอยู่บน Docker (Software) โดยรันอยู่บน Host OS อีกทีหนึ่ง
2. VM ต้องมี Hypervisor มาคอยจัดการ Resource ของ HW อีกทีหนึ่ง
3. Container Utilize Resource ได้ดอย่างมีประสิทธิภาพกว่า VM (CPU, Memory, Storage)
4. VM ต้องมี Guest OS ของตัวเอง ทำให้มีขนาดใหญ่กว่าและหนัก
5. VM เป็นมาตรฐานเก่า และระบบ Cloud ส่วนใหญ่ -> VM

### Containers and VM Together
มีแบบ Stand alone Container บน Host OS และแบบ Server ที่มี VM เป็น Host OS แล้วรัน Container ต่ออีกทีหนึ่ง
In reality มักนำทั้งสองมาใช้ร่วมกัน โดยรัน Docler บน VM อีกทีหนึ่ง
- ช่วยให้จัดการความปลอดภัยและการแบ่งส่วน Server ได้ง่ายขึ้น
- สามารถรันหลาย Application ใน VM ตัวเดียวได้
- Legacy App ก็ยังสามารถรันแบบ VM ดั้งเดิมได้ด้วย

### Docker Engine
#### 1. องค์ประกอบหลักของ Docker Engine
- Docker Daemon (Server) -> เป็นส่วนที่ทำงานอยู่เบื้องหลัง ทำหน้าที่จัดการ Object ต่างๆ เช่น Images, Containers, Networks และ Volumes
- REST API -> เป็นตัวกลางที่ให้โปรแกรมภายนอกส่งคำสั่งเข้ามาคุยกับ Docker Daemon ได้ (Docker Desktop GUI)
- Docker CLI (Client) -> User ใช้พิมพ์คำสั่ง (เช่น docker run) เพื่อสั่งการ Daemon
- Network -> Docker สร้าง Network เพื่อให้ Container ต่างๆคุยกันได้

#### 2. Docker Object
- Image -> เป็น Template ที่ใช้สร้าง Container
- Container -> รันมาจาก Image โดย Container จะมีไฟล์ระบบ, Process, Network และ Resource ของตัวเอง
- Data Volume -> เป็นที่เก็บข้อมูลถาวรของ Container เพื่อให้ข้อมูลไม่หาย
- Network -> เป็นตัวเชื่อมต่อระหว่าง Container

### Docker Architecture
#### 1. องค์ประกอบหลัก
- Client -> User use CLI หรือ Remote API เพื่อส่งคำสั่งไปยัง Docker Daemon
- Docker Host -> ประกอบด้วย Docker Daemon ที่ทำหน้าที่เฝ้ารับคำสั่งและบริหารจัดการ Object ต่างๆ เช่น Containers และ Images
- Registry -> คือที่เก็บ Docker Images เช่น Docker Hub (hub.docker.com) ซึ่งเราสามารถ Pull หรือดาวน์โหลด Image ของคนอื่นมาใช้ได้ เช่น Ubuntu, Nginx, Redis

#### 2. Workflow
- Build -> สร้าง Image ขึ้นมาจากไฟล์ที่ชื่อว่า Dockerfile
- Pull -> ดาวน์โหลด Image จาก Registry (เช่น Docker Hub) มาเก็บไว้ในเครื่อง
- Run -> นำ Image ที่มีอยู่มารันให้กลายเป็น Container

## Docker Workshop # 1
### Hello World Docker
```bash
docker run --name some-nginx -p 9080:80 -d nginx
```
- --name some-nginx -> ตั้งชื่อ Container ว่า some-nginx
- -p 9080:80 -> แมปพอร์ต 80 ของ Container ไปที่พอร์ต
9080 ของเครื่องเรา
- -nginx -> image ที่จะใช้รัน Container (ไม่มี = โหลดจาก hub.docker.com มาใช้)
จากนั้นเปิดเว็บ http://localhost:9080 จะเห็นหน้าเว็บ Nginx

### สร้าง File index.html
หลังจากสร้าง folder html และ index.html แล้ว -> docker run
```bash
docker run --name my-nginx -p 9081:80 -v <your_path>/html:/usr/share/nginx/html nginx
```
เข้า browser http://localhost:9081 จะเห็นหน้าเว็บที

### Docker Commands
Login - เข้าใช้งาน Docker Registry
```bash
docker login
```
Logout - ออกจากระบบ Docker Registry
```bash
docker logout
```
List all image
```bash
docker images
docker image ls
```

### Docker Commands # 2
Search image
```bash
docker search [image-name]
```
Pull image - download image เก็บในเครื่อง
```bash
docker pull [image-name]
```

### Docker Commands # 3
Start container - รัน container จาก image
```bash
docker start <container-id> or <container-name>
```
Stop container - หยุดการทำงาน container
```bash
docker stop <container-id> or <container-name>
```

### Docker Commands # 4
List all running container
```bash
docker ps <options>
```

### Docker Commands # 5 (ยากขึ้น)
Exec Container - เข้าไปใน container ที่รันอยู่
```bash
docker exec -it <container-id> or <container-name> /bin/bash
```
Inspect Container - ดูรายละเอียด container
```bash
docker inspect <container-id> or <container-name>
```
Logs Container - ดู log ของ container
```bash
docker logs <container-id> or <container-name>
```

### Docker Commands # 6
Push image - อัพโหลด image ขึ้น Docker Registry
```bash
docker push [username]/[image-name]:[tag]
```
Tag image - ตั้งชื่อ image ก่อน push
```bash
docker tag [image-id] [username]/[image-name]:[tag]
```

### Docker Commands # 7
Remove Container - ลบ container
```bash
docker rm <container-id> or <container-name>
```
Remove Image - ลบ image
```bash
docker rmi <image-id> or <image-name>
```

### Docker Network
Create Network
```bash
docker network create [network-name]
```
Use Network 
```bash
docker run --network [network-name] [image-name]
```

### Docker File
Dockerfile คือไฟล์ที่ใช้กำหนดขั้นตอนการสร้าง Docker Image
From - จึดเริ่มต้นจาก image ไหน
```Dockerfile
FROM [image-name]:[tag]
```
COPY - คัดลอกไฟล์จากเครื่องเราไปยัง image
```Dockerfile
COPY [source] [destination]
```