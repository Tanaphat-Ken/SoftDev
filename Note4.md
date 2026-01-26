# Docker & Workshop Continue
## Docker Workshop # 2
### Dockerfile
1. EXPOSE -> Port ที่จะเปิด on container
    - EXPOSE 8080
2. COPY -> copy files to container
    - COPY ./html /usr/share/nginx/html
3. ENV -> set environment
    - ENV [key] [value]
4. CMD -> คำสั่งที่จะรันตอน container start
    - CMD command param1 param2
5. WORKDIR -> set dir
    - WORKDIR /app
6. VOLUME -> mount volume (เก็บข้อมูลนอก container)
    - VOLUME /path
7. BUILD -> build image
    - docker build [option] [path] -t [tagname] .


### Docker Compose
- file name -> docker-compose.yml
- docker compose รันในที่ที่ file yaml อยู่ (อ่าน yaml ก่อนเสมอ)
- docker compose เป็นการทำ IAC (Infrastructure as a Code)
- docker compose ใช้ network ที่สร้างเอง
- 1 container = 1 เครื่อง -> เกิดจากการเขียน script, Load image แบบ auto
- docker compose สร้าง container ตาม yaml file (และอื่นๆตามใน yaml)

#### Example
```yaml
services:
  hello:
    image: nginx:alpine
    ports:
      - 9085:85
```
เทียบได้กับ -> docker run --name hello -p 9085:85 -d nginx:alpine
- Services name -> hello
- Image -> nginx:alpine
- Port mapping -> 9085:85

### Docker Compose CLI
Start Docker Compose
```bash
docker compose up -d --build
```

Start Docker Compose with create new - ลบทิ้งแล้วสร้างใหม่ (Container) [Use with CAUTION]
```bash
docker compose up -d --force-recreate
```

List all container - (running container)
```bash
docker compose ps
```

### Docker Compose CLI # 2
Delete all container
```bash
docker compose rm
```

Stop and Delete all container
```bash
docker compose down
```

List all compose
```bash
docker compose list
```

### Docker - Remote Development - Remote Explorer
- Use VS Code Extension (Remote Explorer) เพื่อ remote เข้า Containers
- เหมือนใช้งานในเครื่องนั้นๆ (In Container)
- ทำทุกอย่างได้เหมือน local

## CI/CD Workshop Guide
### Typical Steps
1. Polling Source Code (Git)
2. Build and Unit Test
3. Build Docker Image and Publish to Registry
4. Redeploy K8s Cluster with New Image