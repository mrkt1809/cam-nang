# Cẩm Nang Du Lịch

Sổ tay cá nhân để lưu lại quán ăn ngon, địa điểm tham quan, kinh nghiệm du lịch — đồng bộ giữa điện thoại và máy tính qua Firebase, host miễn phí trên GitHub Pages.

---

## Tính năng

- Lưu địa điểm theo tỉnh thành, phân loại 8 nhóm (ăn uống, cà phê, tham quan, lưu trú, mua sắm, thiên nhiên, giải trí, bar)
- Đánh giá sao, ghi chú kinh nghiệm
- Tìm kiếm + lọc theo tỉnh thành / loại hình
- Mở thẳng Google Maps để điều hướng
- Đồng bộ real-time giữa các thiết bị qua Firebase Firestore
- Xuất / nhập JSON để backup hoặc chia sẻ
- Giao diện tối ưu cho cả desktop và mobile

---

## Setup Firebase (10 phút, miễn phí)

### Bước 1 — Tạo project Firebase

1. Truy cập [console.firebase.google.com](https://console.firebase.google.com)
2. Nhấn **Add project** → đặt tên (ví dụ: `cam-nang-du-lich`) → tắt Google Analytics (không cần) → Create
3. Đợi project tạo xong, vào dashboard

### Bước 2 — Bật Firestore Database

1. Trong dashboard, sidebar trái → **Build** → **Firestore Database**
2. Nhấn **Create database**
3. Chọn location gần Việt Nam: `asia-southeast1 (Singapore)` hoặc `asia-east1 (Tokyo)`
4. Chọn **Start in test mode** (cho phép đọc/ghi free trong 30 ngày — sẽ chuyển sang rules an toàn ở bước sau)
5. Nhấn Enable

### Bước 3 — Cấu hình Security Rules

Vào tab **Rules** trong Firestore, dán nội dung sau và nhấn **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /locations/{document} {
      allow read, write: if true;
    }
  }
}
```

> **Lưu ý:** Rules này cho phép bất kỳ ai có URL của bạn cũng đọc/ghi được data. Phù hợp dùng cá nhân hoặc chia sẻ trong nhóm tin tưởng. Nếu muốn riêng tư hơn, xem mục [Bảo mật nâng cao](#bảo-mật-nâng-cao) ở cuối README.

### Bước 4 — Lấy Firebase Config

1. Vào **Project Settings** (biểu tượng bánh răng ⚙️ cạnh "Project Overview")
2. Cuộn xuống mục **Your apps** → nhấn icon `</>` (Web)
3. Đặt nickname app (ví dụ: `cam-nang-web`) → **Register app**
4. Firebase hiển thị đoạn config kiểu:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "cam-nang-du-lich.firebaseapp.com",
  projectId: "cam-nang-du-lich",
  storageBucket: "cam-nang-du-lich.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123:web:abc..."
};
```

**Copy toàn bộ object `firebaseConfig` này.**

### Bước 5 — Dán Config vào `index.html`

Mở file `index.html`, tìm dòng:

```javascript
// PASTE YOUR FIREBASE CONFIG HERE
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  ...
};
```

Thay thế bằng config vừa copy. Lưu file lại.

---

## Deploy lên GitHub Pages (5 phút)

### Bước 1 — Tạo repository

1. Vào [github.com/new](https://github.com/new)
2. Đặt tên repo (ví dụ: `cam-nang`)
3. Để **Public** (Pages cần Public, hoặc Pro account để dùng Private)
4. Nhấn **Create repository**

### Bước 2 — Upload file

**Cách dễ nhất (qua web):**

1. Trong repo vừa tạo, nhấn **Add file** → **Upload files**
2. Kéo thả 2 file: `index.html` và `README.md`
3. Commit changes

**Hoặc dùng Git command line:**

```bash
git clone https://github.com/YOUR_USERNAME/cam-nang.git
cd cam-nang
# Copy index.html và README.md vào đây
git add .
git commit -m "Initial commit"
git push
```

### Bước 3 — Bật GitHub Pages

1. Vào repo → tab **Settings**
2. Sidebar trái → **Pages**
3. Mục **Source** → chọn branch `main`, folder `/ (root)` → **Save**
4. Đợi 1–2 phút, GitHub Pages sẽ build xong và hiển thị URL kiểu:

```
https://YOUR_USERNAME.github.io/cam-nang/
```

### Bước 4 — Thêm domain vào Firebase (quan trọng!)

Để Firebase chấp nhận request từ GitHub Pages:

1. Quay lại Firebase Console → **Authentication** → **Settings** → tab **Authorized domains**
2. Nhấn **Add domain** → dán `YOUR_USERNAME.github.io` → Add

> Bước này tránh lỗi CORS khi app gọi Firebase từ GitHub Pages.

---

## Cài vào điện thoại như app

Khi đã có URL GitHub Pages:

**iOS (Safari):**
1. Mở URL trên Safari
2. Nhấn nút **Share** (hình vuông + mũi tên)
3. Cuộn xuống → **Add to Home Screen**
4. Đặt tên → Add

**Android (Chrome):**
1. Mở URL trên Chrome
2. Menu 3 chấm → **Add to Home screen** hoặc **Install app**

App sẽ hiện trên màn hình chính, mở ra như app gốc, full màn hình.

---

## Cách dùng

### Thêm địa điểm

- Nhấn nút **+** màu đen ở góc dưới phải
- Điền tên, chọn tỉnh thành + loại hình (bắt buộc)
- Địa chỉ, ghi chú và đánh giá sao là tùy chọn
- Nhấn **Lưu địa điểm**

### Tìm & lọc

- Gõ vào ô tìm kiếm để lọc theo tên / địa chỉ / ghi chú
- Dropdown "Mọi tỉnh thành" để lọc theo tỉnh
- Các pill loại hình bên cạnh để lọc theo nhóm

### Mở Google Maps

Mỗi card có nút **Google Maps** — nhấn vào sẽ mở app Google Maps trên điện thoại, hoặc maps.google.com trên máy tính, đã sẵn vị trí để chỉ đường.

### Backup / Chia sẻ

Nhấn icon **Share** trên thanh đầu trang:
- **Xuất**: copy đoạn JSON, lưu vào Google Drive / Notion / gửi bạn bè
- **Nhập**: dán JSON vào, chọn **Gộp** (thêm vào data hiện tại) hoặc **Thay thế** (xóa hết, thay bằng data mới)

---

## Bảo mật nâng cao (tùy chọn)

Nếu muốn chỉ mình bạn ghi được data, người khác chỉ đọc:

**Cách 1 — Password đơn giản trong code:**

Sửa rules thành:
```
match /locations/{document} {
  allow read: if true;
  allow write: if request.resource.data.secret == "PASSWORD_CỦA_BẠN";
}
```

Sửa code JS để gửi `secret` kèm mỗi request (không an toàn 100% vì password lộ trong code, nhưng đủ để chặn người ngẫu nhiên).

**Cách 2 — Firebase Authentication (an toàn hơn):**

1. Bật Firebase Authentication, chọn Google Sign-In
2. Sửa rules:
```
match /locations/{document} {
  allow read: if true;
  allow write: if request.auth.token.email == "email_cua_ban@gmail.com";
}
```
3. Thêm nút đăng nhập Google vào app

Khi nào cần làm bước này nhắn mình để hỗ trợ tiếp.

---

## Chi phí

Firebase Spark Plan (miễn phí) cho phép:
- 1 GB storage Firestore
- 50,000 lượt đọc / ngày
- 20,000 lượt ghi / ngày

→ Với cá nhân dùng cẩm nang, dư sức dùng vài chục năm cũng không hết hạn mức.

GitHub Pages: hoàn toàn miễn phí cho public repo.

---

## Cấu trúc dữ liệu

Mỗi document trong collection `locations`:

```json
{
  "name": "Bún chả Hương Liên",
  "province": "Hà Nội",
  "category": "food",
  "address": "24 Lê Văn Hưu, Hai Bà Trưng",
  "description": "Chả nướng đậm vị, gọi thêm nem cua bể...",
  "rating": 5,
  "createdAt": 1731648000000
}
```

Các giá trị `category`: `food`, `cafe`, `sight`, `stay`, `shop`, `nature`, `entertain`, `bar`.

---

## Tùy chỉnh

### Thêm tỉnh thành

Trong `index.html`, tìm mảng `PROVS` (gần cuối file), thêm tên mới vào.

### Thêm loại hình

Tìm mảng `CATS`, thêm object mới với `id`, `label`, `icon` (Tabler icon class), `bg`, `col`, `pillBg`, `pillColor`.

Danh sách icon Tabler: [tabler.io/icons](https://tabler.io/icons)

### Đổi màu/font

Sửa CSS variables ở đầu `<style>`:

```css
:root {
  --bg: #F8F5EF;        /* nền */
  --accent: #B8552A;    /* màu nhấn */
  --font-display: 'Fraunces', serif;  /* font tiêu đề */
  --font-body: 'Geist', sans-serif;   /* font nội dung */
}
```

---

## Troubleshooting

**App hiện "Lưu cục bộ" thay vì "Đã đồng bộ"**
→ Chưa dán đúng `firebaseConfig`, hoặc giá trị `apiKey` vẫn là `YOUR_API_KEY`. Kiểm tra lại Bước 5 setup Firebase.

**Console hiện lỗi `Missing or insufficient permissions`**
→ Firestore rules chưa được publish, hoặc rules không cho phép. Quay lại Bước 3 setup Firebase.

**App không load trên GitHub Pages dù chạy ổn local**
→ Kiểm tra Authorized domains trong Firebase Console (Bước 4 deploy).

**Mất hết data khi xóa cache trình duyệt**
→ Chỉ xảy ra khi chưa kết nối Firebase. Khi đã đồng bộ, data ở trên cloud, không lo mất.

---

## Roadmap (gợi ý nâng cấp sau)

- [ ] Đính kèm ảnh cho mỗi địa điểm (dùng Firebase Storage)
- [ ] Tag tùy chỉnh ngoài 8 loại có sẵn
- [ ] Dark mode tự động
- [ ] Bản đồ thực tế hiển thị tất cả pin (cần Mapbox/Google Maps API)
- [ ] Đa người dùng với auth riêng

---

Made with ❤️
