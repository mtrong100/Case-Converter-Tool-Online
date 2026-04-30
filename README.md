# 🔡 Case Converter — Công cụ Chuyển Đổi Chữ Hoa/Thường

> Công cụ trực tuyến miễn phí, hỗ trợ chuyển đổi văn bản tiếng Việt và tiếng Anh sang nhiều kiểu chữ khác nhau. Hoạt động offline nhờ PWA (Progressive Web App).

![PWA](https://img.shields.io/badge/PWA-Ready-5b7fff?style=flat-square&logo=pwa)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)
![HTML](https://img.shields.io/badge/HTML5-Pure-orange?style=flat-square&logo=html5)
![Offline](https://img.shields.io/badge/Offline-Supported-success?style=flat-square)

---

## 📋 Mục lục

- [Tính năng](#-tính-năng)
- [Demo](#-demo)
- [Cấu trúc dự án](#-cấu-trúc-dự-án)
- [Cài đặt & Chạy](#-cài-đặt--chạy)
- [Deploy](#-deploy)
- [PWA](#-pwa---progressive-web-app)
- [Công nghệ sử dụng](#-công-nghệ-sử-dụng)
- [License](#-license)

---

## ✨ Tính năng

### 6 chế độ chuyển đổi

| Chế độ | Ví dụ đầu vào | Kết quả |
|--------|--------------|---------|
| **CHỮ HOA** | `xin chào việt nam` | `XIN CHÀO VIỆT NAM` |
| **chữ thường** | `XIN CHÀO VIỆT NAM` | `xin chào việt nam` |
| **Viết Hoa Mỗi Từ** | `xin chào việt nam` | `Xin Chào Việt Nam` |
| **Viết hoa đầu câu** | `xin chào. việt nam đẹp.` | `Xin chào. Việt nam đẹp.` |
| **xEn KẽM cHỮ** | `Xin Chào` | `xIN cHÀO` |
| **Viết hoa ký tự đầu** | `xin chào việt nam` | `Xin chào việt nam` |

### Tiện ích khác

- 📊 **Thống kê realtime** — đếm ký tự, từ, dòng
- 📋 **Sao chép** vào clipboard chỉ 1 click
- 💾 **Tải xuống** kết quả dạng `.txt`
- 📴 **Banner offline** — thông báo khi mất kết nối
- 📲 **Prompt cài đặt** — gợi ý cài app khi có thể
- 🔗 **URL param** — preselect chế độ qua `?mode=uppercase`

---

## 🚀 Demo

Mở file `index.html` trực tiếp trên trình duyệt, hoặc deploy lên hosting HTTPS để dùng đầy đủ tính năng PWA.

> ⚠️ **Lưu ý:** Service Worker và tính năng cài đặt PWA **yêu cầu HTTPS**. Mở file local (`file://`) sẽ không kích hoạt được Service Worker.

---

## 📁 Cấu trúc dự án

```
case-converter-pwa/
├── index.html          # Trang chính — toàn bộ UI và logic
├── manifest.json       # Web App Manifest — cấu hình PWA
├── sw.js               # Service Worker — cache & offline
├── icons/
│   ├── icon-192.png    # Icon PWA (192×192)
│   └── icon-512.png    # Icon PWA (512×512)
└── README.md           # Tài liệu dự án
```

---

## ⚙️ Cài đặt & Chạy

### Chạy local (không có PWA)

Chỉ cần mở file trực tiếp:

```bash
# Clone hoặc giải nén project
cd case-converter-pwa

# Mở trực tiếp trên trình duyệt
open index.html
# hoặc double-click vào index.html
```

> Tính năng cơ bản (chuyển đổi, copy, download) hoạt động bình thường. Service Worker **không** chạy trên `file://`.

### Chạy local với PWA đầy đủ

Cần một HTTP server local hỗ trợ HTTPS hoặc `localhost`:

```bash
# Dùng Python (có sẵn trên macOS/Linux)
cd case-converter-pwa
python3 -m http.server 8080
# Truy cập: http://localhost:8080

# Hoặc dùng Node.js (cần cài npx)
npx serve .
# Truy cập theo địa chỉ hiện ra

# Hoặc dùng VS Code extension "Live Server"
# Click chuột phải vào index.html → Open with Live Server
```

---

## 🌐 Deploy

Project là **static site** thuần — không cần server, không cần build. Chỉ cần upload lên hosting hỗ trợ HTTPS.

### GitHub Pages

```bash
# 1. Tạo repository mới trên GitHub
# 2. Push code lên
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/<username>/<repo>.git
git push -u origin main

# 3. Vào Settings → Pages → Source: Deploy from branch → main
# 4. Truy cập: https://<username>.github.io/<repo>
```

### Netlify

```bash
# Kéo thả thư mục case-converter-pwa/ vào https://app.netlify.com/drop
# Netlify tự cấp domain HTTPS miễn phí
```

### Vercel

```bash
npm i -g vercel
cd case-converter-pwa
vercel
# Làm theo hướng dẫn, Vercel tự deploy và cấp HTTPS
```

---

## 📲 PWA — Progressive Web App

Project hỗ trợ cài đặt như ứng dụng native trên cả desktop và mobile.

### Cách hoạt động

```
Trình duyệt
    │
    ├── Tải index.html
    ├── Đăng ký sw.js (Service Worker)
    │       │
    │       ├── [Install] Cache toàn bộ assets vào CacheStorage
    │       ├── [Activate] Xóa cache cũ
    │       └── [Fetch]  Cache-first strategy
    │                     ├── Có cache → trả về ngay (offline OK)
    │                     └── Không cache → fetch từ mạng + lưu cache
    │
    └── Đọc manifest.json
            ├── Tên app, icon, màu theme
            ├── display: standalone (giống app native)
            └── shortcuts (shortcut nhanh trên Android)
```

### Chiến lược cache (Cache-First)

```
Request đến
    │
    ├── Có trong Cache? ──Yes──→ Trả về từ Cache (nhanh, offline OK)
    │
    └── Không có ──────────────→ Fetch từ Network
                                      │
                                      ├── Thành công → Lưu vào Cache + Trả về
                                      └── Thất bại   → Fallback index.html
```

### Cài đặt trên thiết bị

| Nền tảng | Cách cài |
|----------|---------|
| **Android (Chrome)** | Banner tự hiện, hoặc menu ⋮ → *Thêm vào màn hình chính* |
| **iOS (Safari)** | Nút chia sẻ → *Thêm vào màn hình chính* |
| **Windows/Mac (Chrome/Edge)** | Icon ⊕ trên thanh địa chỉ, hoặc menu → *Cài đặt ứng dụng* |

### Cập nhật cache

Khi muốn buộc người dùng tải lại assets mới, thay đổi tên cache trong `sw.js`:

```js
// sw.js
const CACHE_NAME = 'case-converter-v2'; // tăng version lên
```

---

## 🛠 Công nghệ sử dụng

| Công nghệ | Mục đích |
|-----------|---------|
| **HTML5** | Cấu trúc trang |
| **CSS3** | Giao diện, animation, responsive |
| **Vanilla JavaScript** | Logic chuyển đổi, PWA |
| **Service Worker API** | Cache, offline support |
| **Web App Manifest** | Cấu hình PWA, installable |
| **Cache API** | Lưu trữ assets offline |
| **Clipboard API** | Sao chép kết quả |
| **Google Fonts** | Be Vietnam Pro, Playfair Display |

> Không dùng framework, không có build step, không có dependency — chỉ là HTML/CSS/JS thuần.

---

## 🌍 Hỗ trợ trình duyệt

| Trình duyệt | Hỗ trợ |
|-------------|--------|
| Chrome 67+ | ✅ Đầy đủ (PWA + Install) |
| Edge 79+ | ✅ Đầy đủ (PWA + Install) |
| Firefox 65+ | ✅ PWA (không có install prompt) |
| Safari 16.4+ | ✅ PWA + Add to Home Screen |
| Samsung Internet | ✅ Đầy đủ |

---

## 📄 License

MIT License — tự do sử dụng, chỉnh sửa và phân phối.

---

<p align="center">Làm với ❤️ · Hỗ trợ tiếng Việt đầy đủ · Hoàn toàn miễn phí</p>
