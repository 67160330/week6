# ETL Lab Report

Student ID: [67160330]
Name: Natthachai Deechaiya

## 1. Data Quality Problems Found
- พบ order_id ซ้ำซ้อน
- ฟอร์แมตวันที่ใน order_date ผสมกันหลายรูปแบบ
- ค่าตัวเลขผิดปกติ เช่น qty <= 0, unit_price < 0, discount_pct อยู่นอกช่วง 0-100
- ไม่พบ customer_id หรือ product_id ในตาราง Master
- ตัวอักษร province และ status พิมพ์เล็ก-ใหญ่ไม่สม่ำเสมอ

## 2. Cleaning / Transformation Rules
- ลบรายการ ID ที่ซ้ำซ้อน
- แปลงวันที่ด้วย pd.to_datetime ให้เป็นรูปแบบมาตรฐาน YYYY-MM-DD
- แยกแถวที่ผิดเงื่อนไขไปบันทึกที่ output/rejects.csv
- กรองเฉพาะรายการที่มีสถานะ paid และ completed
- คำนวณ sales_amount = (qty * unit_price) - discount_amount

## 3. Rejected Records
จำนวน: 9

เหตุผลหลัก: วันที่ไม่ถูกต้อง, ยอดขาย/ส่วนลดติดลบ, ค่าอ้างอิง ID ไม่พบใน Master Data

## 4. ETL Validation
- Valid transformed rows: 100
- Warehouse rows: 100
- Duplicate order_id: 0
- Source total sales: 192074.66
- Warehouse total sales: 192074.66
- Validation status: PASS

## 5. Idempotency Test
จำนวน fact_sales หลัง run ครั้งที่ 1: 100

จำนวน fact_sales หลัง run ครั้งที่ 2: 100

อธิบายผล: จำนวนแถวใน fact_sales เท่าเดิมเนื่องจากมีการกำหนด order_id เป็น PRIMARY KEY และใช้คำสั่ง INSERT OR REPLACE ในการบันทึกทับข้อมูลเดิม
