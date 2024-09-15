```
Table User [note: "ตาราง User ประกอบด้วยข้อมูลของผู้ใช้งาน"] {
  id            Int        [note: "รหัสบัญชีผู้ใช้งานที่ไม่ซ้ำกัน"]
  role          Enum('ADMIN', 'USER')  [note: "สถานะของบัญชีผู้ใช้งาน (ADMIN หรือ USER)"]
  username      Varchar(30) Unique  [note: "ชื่อผู้ใช้งานที่ไม่ซ้ำกันสำหรับการเข้าสู่ระบบ"]
  password      Varchar(72)  [note: "รหัสผ่านที่ถูกแฮช"]
  email         Varchar(70)  [note: "อีเมลของผู้ใช้งาน"]
  phone         Varchar(10)  [note: "เบอร์โทรศัพท์ของผู้ใช้งาน"]
  created_at    DateTime     [note: "วันที่สร้างบัญชี"]
  updated_at    DateTime     [note: "วันที่แก้ไขบัญชีล่าสุด"]
}


Table Reserved [note: "ตาราง Reserved ประกอบด้วยข้อมูลการจองที่จองสำหรับจอง"] {
  id              Int        [note: "รหัสการจองที่ไม่ซ้ำกัน"]
  vehicleNumber_id Int [ref: > VehicleNumber.id, note: "รหัสรถที่จอง"]  
  startDate       DateTime   [note: "วันเริ่มต้นและเวลาของการจอง"]
  endDate         DateTime   [note: "วันสิ้นสุดและเวลาของการจอง"]
  reserverDate    DateTime   [note: "วันที่และเวลาที่ทำการจอง"]
  user_id         Int        [note: "รหัสผู้ใช้งานที่ทำการจอง"]
  status          Enum('RESERVED', 'USED', 'CANCEL')  [note: "สถานะของการจอง"]
  zoneName        Varchar(50)  [note: "ชื่อโซนจอดรถ"]
  location        Varchar(100) [note: "สถานที่ของโซนจอดรถ"]
  parking_zone_id Int [ref: > ParkingZone.id, note: "รหัสโซนจอดรถ"]  
  created_by_id   Int [ref: > User.id, note: "รหัสผู้ใช้งานที่สร้างการจอง"]  
  created_at      DateTime   [note: "วันที่สร้างการจอง"]
  updated_at      DateTime   [note: "วันที่แก้ไขการจองล่าสุด"]
}


Table VehicleNumber [note: "ตาราง VehicleNumber ประกอบด้วยข้อมูลของยานพาหนะ"] {
  id            Int        [note: "รหัสยานพาหนะที่ไม่ซ้ำกัน"]
  vehicleNumber Varchar    [note: "หมายเลขทะเบียนยานพาหนะ"]
  brand         Varchar    [note: "ยี่ห้อหรือรุ่นของยานพาหนะ"]
  model         Varchar    [note: "โมเดลของยานพาหนะ"]
  user_id       Int [ref: > User.id, note: "รหัสผู้ใช้งานที่เป็นเจ้าของยานพาหนะ"]  
  created_at    DateTime   [note: "วันที่สร้างข้อมูลยานพาหนะ"]
  updated_at    DateTime   [note: "วันที่แก้ไขข้อมูลยานพาหนะล่าสุด"]
}


Table ParkingZone [note: "ตาราง ParkingZone ประกอบด้วยข้อมูลของโซนจอดรถ"] {
  id            Int        [note: "รหัสโซนจอดรถที่ไม่ซ้ำกัน"]
  zoneName      Varchar(50)  [note: "ชื่อโซนจอดรถ"]
  location      Varchar(100) [note: "สถานที่ของโซนจอดรถ"]
  coordinates   Json     [note: "พิกัดของโซนจอดรถ (lat,lng) สำหรับการสร้างพื้นที่"]
  created_at    DateTime   [note: "วันที่สร้างข้อมูลโซนจอดรถ"]
  updated_at    DateTime   [note: "วันที่แก้ไขข้อมูลโซนจอดรถล่าสุด"]
}

Table ParkingZoneImage [note: "ตาราง ParkingZoneImage ประกอบด้วยข้อมูลของภาพสำหรับโซนจอดรถ"] {
  id              Int        [note: "รหัสภาพที่ไม่ซ้ำกัน"]
  parking_zone_id Int [ref: > ParkingZone.id, note: "รหัสโซนจอดรถที่เกี่ยวข้อง"]  
  imageUrl        Varchar(255) [note: "URL ของภาพ"]
  description     Varchar(255) [note: "คำอธิบายเกี่ยวกับภาพ"]
  created_at      DateTime   [note: "วันที่สร้างข้อมูลภาพ"]
  updated_at      DateTime   [note: "วันที่แก้ไขข้อมูลภาพล่าสุด"]
}

Table ParkingPackage [note: "ตาราง ParkingPackage ประกอบด้วยข้อมูลราคาสำหรับโซนจอดรถ"] {
  id                Int        [note: "รหัสแพ็คเกจจอดรถที่ไม่ซ้ำกัน"]
  parking_zone_id   Int [ref: > ParkingZone.id, note: "รหัสโซนจอดรถ"]  
  price             Decimal(10, 2)  [note: "ราคาของแพ็คเกจจอดรถ"]
  currency          Varchar(3)  [note: "รหัสสกุลเงิน เช่น USD, EUR"]
  thumbnailImageUrl Varchar(255) [note: "URL ของภาพตัวอย่าง"]
  created_at        DateTime   [note: "วันที่สร้างแพ็คเกจจอดรถ"]
  updated_at        DateTime   [note: "วันที่แก้ไขแพ็คเกจจอดรถล่าสุด"]
}

Table ReservedParkingPackage [note: "ตาราง ReservedParkingPackage สร้างความสัมพันธ์ many-to-many ระหว่าง Reserved และ ParkingPackage"] {
  reserved_id     Int [ref: > Reserved.id, note: "รหัสการจอง"]  
  parking_package_id Int [ref: > ParkingPackage.id, note: "รหัสแพ็คเกจจอดรถ"]  
  created_at      DateTime   [note: "วันที่สร้างความสัมพันธ์"]
  updated_at      DateTime   [note: "วันที่แก้ไขความสัมพันธ์ล่าสุด"]
}

```
