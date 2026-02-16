# Workshop Automate testing with Jenkins on k8s

## Workshop # 1 เตรียม Project
### Step 1
- uv add fastapi --extra=standard
- uv add robotframework-requests
- uv add pytest
### Step 2 create file my_lib.py
```python
def add(a, b):
    return a + b
```
### Step 3 create file tests/test_my_lib.py
```python
from my_lib import add

def test_add_positive_numbers():
    result = add(2, 3)
    assert result == 5

def test_failure_example():
    result = add(2, 2)
    assert result == 5
```
### Step 4 run unit test with pytest
- pytest

## Workshop # 2 การสร้าง REST API ด้วย fastapi
### Step 1 create file app.py (Using FASTAPI)
```python
@app.get("/healthz") # Health Check Endpoint
async def health_check():
    return {"status": "ok"}

@app.get("/sayname") # Print Hello, NAMEE!
async def say_name():
    return {"message": f"Hello, NAMEE!"}

@app.get("/add") # Add two numbers
async def do_add(a: int, b: int):
    return {"value": a + b}
```
### Step 2 run API server
- uv run fastapi dev app.py

## Workshop # 3 การสร้าง Robot Framework Test Cases
### Step 1 create file tests/test_api.robot
ในส่วนของ Test Case นี้ มีเพียงแค่ 1 ตัวอย่างเท่านั้น แต่สามารถเพิ่ม Test Case อื่นๆ ได้ตามต้องการ
```robot
*** Settings ***
Library    RequestsLibrary
Library    Collections

*** Variables ***
${BASE_URL}    http://localhost:8000

*** Test Cases ***
Check system message (GET)
    [Documentation]    ทดสอบดึงข้อมูล
    Create Session    api_session    ${BASE_URL}
    ${response}=    Get On Session    api_session    url=/sayname

    # ตรวจสอบ Status Code ว่าเป็น 200
    Status Should Be    200    ${response}

    # ตรวจสอบค่าใน JSON Response ว่าตรงกับที่คาดหวัง
    ${message}=    Set Variable    ${response.json()['message']}
    Should Be Equal As Strings    ${message}    Hello, NAMEE!

    Log To Console    \nSystem say: ${message}
```
### Step 2 add more test cases
### Step 3 run robot test cases
- uv run robot -d results tests/ # จะเก็บผลลัพธ์ไว้ในโฟลเดอร์ results

## Workshop # 4 Add uv container to Pod Template
### Add Container at cloud configuration -> Pod Template
- Name : uv
- Docker Image : astral/uv:debian-slim
- Allocate pseudo-TTY : true
- (Add Jmeter Container ตามขั้นตอนเดิม)

## Workshop # 5 Add unit test stage
### Step 1 Edit Jenkinsfile add unit test stage
### Step 2 Add post process ต่อท้าย stages เพื่อเก็บผลลัพธ์ของ unit test

## Workshop # 6 Modify Dockerfile 
### Step 1 Edit Dockerfile
```Dockerfile
FROM astral/uv:debian-slim
WORKDIR /app
COPY app.py .
COPY my_lib.py .
COPY pyproject.toml .
RUN uv sync
EXPOSE 8000
CMD ["uv", "run", "fastapi", "run", "app.py"] # เปลี่ยนจาก uv run dev app.py เป็น uv run fastapi run app.py เพื่อให้เหมาะกับการใช้งานจริง (Production)
```
### Step 2 Edit Jenkinsfile -> แก้ไข Deploy Stage ให้มี rollout
- rollout ช่วยให้มั่นใจว่า deploy เสร็จแล้ว ก่อนจะเข้าไปขั้นต่อไป
### Step 3 Edit Jenkinsfile -> เพิ่ม Stage API Testing โดยใช้ container uv
### Step 4 Edit Jenkinsfile -> เพิ่ม post process เพื่อเก็บผลลัพธ์ของ API Testing (Robot)
##
- การวาง Pipeline ควรที่จะเริ่มจาก Build -> Unit Test -> Deploy -> API Testing (หรือ Integration Testing) เพื่อให้มั่นใจว่าโค้ดที่ถูก deploy ขึ้นไปนั้นทำงานได้