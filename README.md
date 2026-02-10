# 🌐 360° Image Viewer สำหรับ AppSheet

ระบบดูภาพ 360 องศาจาก Google Drive ผ่าน AppSheet

## 📋 คุณสมบัติ

- ✅ ดูภาพ 360° แบบ Interactive (ลาก, ซูม, หมุน)
- ✅ รองรับการใช้งานบนมือถือ (Gyroscope)
- ✅ ดึงภาพจาก Google Drive ด้วย Image ID
- ✅ เชื่อมต่อกับ AppSheet และ Google Sheets
- ✅ Auto-rotate ภาพ (ปรับได้)
- ✅ Fullscreen mode

## 🚀 วิธีติดตั้ง

### 1. ติดตั้งใน GitHub

1. Fork หรือ Clone repository นี้
2. ไปที่ **Settings** → **Pages**
3. เลือก **Source**: Deploy from a branch
4. เลือก **Branch**: main (หรือ master) → **folder**: / (root)
5. กด **Save**
6. รอสักครู่ จะได้ URL: `https://YOUR_USERNAME.github.io/YOUR_REPO/`

### 2. ตั้งค่า Google Sheets

1. เปิด Google Sheets ที่เชื่อมกับ AppSheet
2. ไปที่ **Extensions** → **Apps Script**
3. คัดลอกโค้ดจากไฟล์ `appsheet-image-handler.gs` วางลงไป
4. แก้ไข URL ใน function `create360ViewerLink`:
   ```javascript
   return `https://YOUR_USERNAME.github.io/YOUR_REPO/?id=${fileId}`;
   ```
5. กด **Save** → **Run** → อนุญาต permissions
6. Refresh Google Sheets จะเห็นเมนู **🌐 360° Image Tools**

### 3. ตั้งค่า Google Sheets Columns

สร้าง columns ใน Google Sheets:

| Column A | Column B | Column C |
|----------|----------|----------|
| Image_URL | Image_ID | Viewer_Link |
| (URL จาก AppSheet) | (Auto-generated) | (Auto-generated) |

### 4. ตั้งค่า Google Drive

**สำคัญ!** ภาพใน Google Drive ต้อง:
1. เปิดการแชร์: **Anyone with the link** → **Viewer**
2. ไฟล์ภาพต้องเป็นรูปแบบ **Equirectangular** (360°)

วิธีแชร์:
1. คลิกขวาที่ไฟล์ใน Google Drive
2. เลือก **Share** → **General access** 
3. เลือก **Anyone with the link** → **Viewer**
4. กด **Done**

## 📱 วิธีใช้งาน

### จาก AppSheet:
1. อัพโหลดภาพ 360° ผ่าน AppSheet
2. ระบบจะบันทึก URL ลง Google Sheets
3. Apps Script จะสร้าง Image ID และ Viewer Link อัตโนมัติ
4. คลิกที่ Viewer Link เพื่อดูภาพ 360°

### การใช้งาน Viewer:
- 🖱️ **ลากเมาส์**: หมุนภาพ
- 🔍 **Scroll**: ซูมเข้า/ออก
- 📱 **สัมผัส**: ปัดนิ้วเพื่อหมุน
- 🧭 **Gyroscope**: เอียงมือถือเพื่อมอง
- ⛶ **Fullscreen**: คลิกปุ่มมุมขวาล่าง

## 🔧 Custom Functions ใน Google Sheets

### 1. `=GET_IMAGE_ID(url)`
ดึง Image ID จาก Google Drive URL
```
=GET_IMAGE_ID(A2)
```

### 2. `=CREATE_DIRECT_LINK(fileId)`
สร้าง Direct Image URL
```
=CREATE_DIRECT_LINK(B2)
```

### 3. `=CREATE_360_LINK(fileId)`
สร้าง 360° Viewer Link
```
=CREATE_360_LINK(B2)
```

## 🛠️ ปรับแต่ง

### ปิดการหมุนอัตโนมัติ
แก้ไขใน `index.html`:
```javascript
"autoRotate": 0, // เปลี่ยนจาก -2 เป็น 0
```

### ปรับความเร็วหมุน
```javascript
"autoRotate": -5, // เลขติดลบ = หมุนซ้าย, บวก = หมุนขวา
```

### ปรับมุมมองเริ่มต้น
```javascript
"pitch": 0,   // มุมแนวตั้ง (-90 ถึง 90)
"yaw": 180,   // มุมแนวนอน (0 ถึง 360)
"hfov": 100,  // Field of view เริ่มต้น
```

### ปรับระดับซูม
```javascript
"minHfov": 50,  // ซูมเข้าสุด
"maxHfov": 120, // ซูมออกสุด
```

## 📸 วิธีถ่ายภาพ 360°

### บนมือถือ:
- **Android**: Google Street View app
- **iOS**: Google Street View app หรือ 360 Panorama

### บนกล้อง:
- Ricoh Theta
- Insta360
- GoPro Max

### บนคอมพิวเตอร์:
- Blender (3D renders)
- Hugin (stitching)
- PTGui

## 🌟 ตัวอย่าง URL

### URL สำหรับดูภาพ:
```
https://your-username.github.io/your-repo/?id=1ABC-XYZ123-FILE_ID
```

### URL Parameters:
- `id`: Google Drive File ID (required)

## ❗ การแก้ปัญหา

### ภาพไม่โหลด
1. ✅ ตรวจสอบ Image ID ถูกต้อง
2. ✅ ตรวจสอบไฟล์แชร์เป็น "Anyone with the link"
3. ✅ ตรวจสอบไฟล์เป็นภาพ 360° (Equirectangular)
4. ✅ ลองเปิด Direct URL ใน browser: `https://drive.google.com/uc?export=view&id=YOUR_FILE_ID`

### ภาพผิดเพี้ยน
- ภาพต้องเป็น Equirectangular projection (ภาพสี่เหลี่ยม 2:1 ratio)
- ตรวจสอบ metadata ของภาพ

### ใช้งานบนมือถือไม่ได้
- ตรวจสอบ HTTPS (GitHub Pages ใช้ HTTPS อยู่แล้ว)
- อนุญาตให้ browser เข้าถึง sensors

## 📦 เทคโนโลยีที่ใช้

- [Pannellum](https://pannellum.org/) - 360° Image Viewer
- Google Apps Script - Automation
- GitHub Pages - Hosting
- Google Drive - Image Storage
- AppSheet - Mobile App

## 📄 License

MIT License - ใช้งานได้อย่างอิสระ

## 🤝 Contributing

Pull requests are welcome!

## 📧 ติดต่อ

หากมีปัญหาการใช้งาน สามารถ:
- เปิด Issue ใน GitHub
- ดู [Pannellum Documentation](https://pannellum.org/documentation/overview/)
- ดู [AppSheet Community](https://community.appsheet.com/)

---

Made with ❤️ for AppSheet Users
