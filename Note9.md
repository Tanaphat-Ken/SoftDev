# Load Testing with jMeter
## How to do Load Testing?
1. Open jMeter
2. Create Thread Group
3. Add Sampler (HTTP Request)
   - http -> Port 80
   - https -> Port 443
4. Add Listener (View Results Tree)
   - Others -> Summary Report, Graph Results, etc.
5. Run Test

## Debugging
Check ว่าเป็นไปตามนี้ไหม
- User ส่ง request ไปยัง Ingress ได้ไหม
- Ingress ส่ง request ไปยัง Service ได้ไหม
- Service ส่ง request ไปยัง Pod ได้ไหม
- Pod ทำงานได้ไหม

## ETC.
- FastAPI จะเปลี่ยน port เป็น 8000 โดยอัตโนมัติ ถ้าไม่กำหนด port เอง