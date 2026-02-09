# Workshop Jenkins on Kubernetes

## Workshop # 1 การสร้าง Jenkins user
- สร้าง Jenkins user สำหรับใช้งาน Jenkins
- ตอนสร้างให้ใช้ nsXXXXXXXX เป็น username ไว้ใช้ login Jenkins + namespace

## Workshop # 2 การตสร้าง Jenkins Credential
- Credential เอาไว้เก็บข้อมูลที่ต้องใช้ในการเชื่อมต่อกับระบบอื่นๆ
- Password ใช้ token ที่ gen จาก gitlab personal access token (ที่ดีควร expire 15-30 วัน ไม่ควรใช้ token ถาวร และไม่ควรเกิน 45 วัน)
- ในที่นี่ใช้ token ที่สามารถ access ได้ 3 ส่วนคือ read_repository, read registry, write_registry
```
Kind = Username with password
Scope = Global
Username = Email ที่ใช้ login gitlab
Password = รหัสผ่านสำหรับ gitlab (ใช้ personal access token)
ID = ชื่อตัวแปรสำหรับการอ้างถึงใน script Jenkinsfile
Description = คำอธิบาย
```

## Workshop # 3 การสร้าง Jenkins Job / Item
- สร้าง new item with name : jbXXXXXXXX
- เลือก Pipeline แล้วใส่ script ให้มี 2 Stages ไว้ทดสอบ
- การสั่งให้ทำงานแบบ manual ให้กดที่ Build Now (เข้ามาใน Job ก่อน)

## Workshop # 4 Setup Kubernetes on Jenkins
- ### ปิด Cloud ใน Jenkins
- New cloud with name : cdXXXXXXXX
- Type = Kubernetes
- กรอกค่า Name และ Kubernetes URL
- ใส่ Credentials + เลือก Websocket + Jenkins URL
- Test Connection
- กด Save
- ### สร้าง Pod Template
- Name : ptXXXXXXXX (Best Practive Name ต้องเหมือนกับ Labels)
- Namespace : knXXXXXXXX (Kubernetes Namespace ไว้กันการชนกันกับ namespace อื่น (คนอื่น))
- Labels : ptXXXXXXXX (Best Practive Name ต้องเหมือนกับ Name)
- Add Container
- Name : buildkit (ชื่อตรงนี้ซ้ำคนอื่นได้ เพราะอยู่คนละ namespace)
- Docker Image : moby/buildkit:latest
- Command to run : ไม่ใช้
- Arguments to pass to the command : --addr tcp://0.0.0.0:1234
- At advanced : Use Run in privileged mode
- Add another Container
- Name : buildctl
- Docker Image : moby/buildkit:latest
- Command to run : sleep
- Arguments to pass to the command : 9999999
- Environment Variables
```
Key : BUILDKIT_HOST
Value : tcp://localhost:1234
```
- Add another Container
- Name : kubectl
- Docker Image : alpine/kubectl
- Command to run : sleep
- Arguments to pass to the command : 9999999
- ### Node Selector ใส่ node-role.kubernetes.io/worker: "worker"
- save
- (Kubernetes deploy บน worker node เท่านั้น ไม่ใช่บน control plane)

## Workshop # 5 Setup Kubernetes namespace
- สร้างที่ kubernetes dashboard
- ใช้ create from input
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: knXXXXXXXX
```

## Workshop # 6 Jenkins CI/CD with Gitlab
- สร้าง Jenkins File with pipeline script include agent, environment, stages
- สร้าง 2 stages โดยใช้ container buildkit กับ kubectl
- Push then build now manually on Jenkins (ต้องทำการใส่ Pipeline script SCM แบบ Git ด้วยและใส่ repository gitlab with Branch */main)

## Workshop # 7 สร้าง Custom Image
- ต้องมี Dockerfile และ docker image ด้วย
- แล้ว push ขึ้น registry (gitlab registry)

## Workshop # 8 Jenkins CI/CD with docker
- การสร้าง Environment
```
APP_NAME = ชื่อ deployment ใน kubernetes
IMAGE_NAME = ชื่อ docker image พร้อม registy server
NAMESPACE = ชื่อ namespace ใน kubernetes
CREDENTIAL_ID = ชื่อ Jenkins Credential ที่สร้างไว้
```
- เพิ่มเติม Stage การ Build ใน Jenkinsfile (การ Build ที่กีควรมีเลข version ด้วย เช่น 1.0.$BUILD_NUMBER)
- Push ขึ้น gitlab
- Then manually build now on Jenkins

## Workshop # 9 Kubernetes deployment yml
- สร้าง secret จากเว็บแปลง base64
- สร้าง hello-deploy.yml โดยที่สำคัญคือใส่ data: .dockerconfigjson: <base64 ที่แปลงมา>
- แก้ yml ไปเพิ่ม deployment, service, network ingress ตามต้องการ
- push ขึ้น gitlab

## Workshop # 10 Jenkins automate CI/CD with gitlab docker and kubernetes
- Config ให้ใช้ Poll SCM (ใน Jenkins)
- ระบุเววลาเช่น H/5 * * * * (ทุก 5 นาที)
- แก้ไข Jenkinsfile ให้เพิ่ม stage การ deploy - kubectl (hello-deploy.yml)
- push ขึ้น gitlab
- Jenkins จะดึง code มา build และ deploy ให้อัตโนมัติ ทุก 5 นาที (ถ้ามีการเปลี่ยนแปลงใน gitlab)