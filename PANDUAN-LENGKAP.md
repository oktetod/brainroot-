# 🇮🇹 PANDUAN ITALIAN BRAINROT STREAMER

## STRUKTUR FILE

```
📁 vercel-game/
   ├── index.html       ← Game utama (deploy ke Vercel)
   └── vercel.json      ← Config Vercel

📁 windows-stream/
   ├── config.ps1           ← ⚠️ EDIT INI DULU!
   ├── 1-install-tools.ps1  ← Install semua tools
   ├── 2-start-stream.ps1   ← Mulai stream (sekali)
   ├── 3-autostream-24x7.ps1 ← Stream 24/7 auto-restart
   └── 4-stop-stream.ps1    ← Stop stream
```

---

## LANGKAH 1: DEPLOY KE VERCEL

### Cara Paling Mudah (Drag & Drop)
1. Buka https://vercel.com → Login/Daftar
2. Klik **"Add New Project"**
3. Pilih **"Deploy from template"** → atau klik "Browse" dan upload folder `vercel-game/`
4. Vercel otomatis detect sebagai static site
5. Klik **Deploy**
6. Copy URL-nya, contoh: `https://italian-brainrot.vercel.app`

### Cara via Git (Lebih Pro)
```bash
# Di terminal/cmd
npm i -g vercel
cd vercel-game/
vercel --prod
```

---

## LANGKAH 2: EDIT CONFIG

Buka file `config.ps1` dan isi:

```powershell
$YOUTUBE_STREAM_KEY = "xxxx-xxxx-xxxx-xxxx-xxxx"
$VERCEL_URL = "https://nama-app-kamu.vercel.app"
```

### Cara Dapat YouTube Stream Key:
1. Buka https://studio.youtube.com
2. Klik **"Go Live"** (ikon kamera)
3. Pilih **"Stream"** tab
4. Copy **Stream Key** (format: xxxx-xxxx-xxxx-xxxx-xxxx)

---

## LANGKAH 3: INSTALL TOOLS DI RDP

1. Buka PowerShell sebagai **Administrator**
2. Jalankan:
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
.\1-install-tools.ps1
```

Ini akan install:
- ✅ Chocolatey (package manager)
- ✅ FFmpeg (untuk streaming)
- ✅ Google Chrome (untuk buka game)
- ✅ Virtual Display Driver (untuk streaming saat RDP disconnect)

---

## LANGKAH 4: MULAI STREAMING

### Test sekali dulu:
```powershell
.\2-start-stream.ps1
```
Cek di YouTube Studio → "Manage" → lihat apakah stream masuk.

### Untuk streaming 24/7 (auto-restart kalau crash):
```powershell
.\3-autostream-24x7.ps1
```

---

## LANGKAH 5: STREAMING SAAT RDP DISCONNECT

Masalah: kalau kamu disconnect dari RDP, layar virtual hilang → FFmpeg error.

### Solusi (pilih salah satu):

**Opsi A - Disconnect tanpa logout (paling simpel):**
```powershell
# Jalankan ini sebelum disconnect RDP
# Script streaming harus sudah jalan dulu!
tscon $env:SESSIONNAME /dest:console
```
> Catatan: resolusi mungkin berubah saat disconnect

**Opsi B - Virtual Display Driver (permanen, recommended):**
1. Install dari: https://github.com/itsmikethetech/Virtual-Display-Driver
2. Setelah install, ada monitor virtual 1280x720 yang selalu ada
3. Set Chrome & FFmpeg ke monitor virtual itu
4. Bisa disconnect RDP kapanpun, stream tetap jalan!

**Opsi C - Jadikan Windows Service dengan NSSM:**
```powershell
# Download NSSM dulu: https://nssm.cc/download
# Lalu:
nssm install BrainrotStream powershell.exe "-File C:\BrainrotStream\3-autostream-24x7.ps1"
nssm start BrainrotStream
```
> Stream otomatis jalan saat Windows start, bahkan tanpa login!

---

## TROUBLESHOOTING

### ❌ FFmpeg error "Could not find monitor"
- Pastikan resolusi desktop = 1280x720
- Coba: Display Settings → Change resolution manually

### ❌ "Stream key invalid"
- Pastikan stream key tidak expired (YouTube reset tiap 24 jam kalau tidak aktif)
- Generate stream key baru di YouTube Studio

### ❌ Chrome tidak muncul
- Cek path Chrome di `config.ps1`
- Coba install ulang Chrome

### ❌ Stream lag/buffering
- Turunkan quality di config: `$STREAM_QUALITY = "low"`
- Atau ganti preset FFmpeg ke "ultrafast"

### ❌ "Black screen" di YouTube
- Tunggu 30-60 detik, YouTube butuh waktu untuk process
- Pastikan game di Chrome ter-load dulu sebelum FFmpeg jalan

---

## TIPS BIAR STREAM RAME

1. **Judul YouTube:** "🇮🇹 Italian Brainrot LIVE - Tralalero vs Bombardino 24/7 🐱🐊"
2. **Thumbnail:** Screenshot game dengan teks besar
3. **Tags:** italian brainrot, tralalero, bombardino, brainrot, live stream
4. **Jadwal:** Gunakan YouTube Scheduler biar stream mulai otomatis
5. **Kategori:** Gaming atau Entertainment

---

## ARSITEKTUR SISTEM

```
[Vercel CDN]
    ↓ (HTTPS)
[Chrome Kiosk - 1280x720]
    ↓ (Desktop capture)
[FFmpeg gdigrab - CPU encode]
    ↓ (RTMP)
[YouTube Live Stream 24/7]
```

CPU usage estimasi: ~40-60% untuk encode 1080p/30fps
RAM usage: Chrome ~300MB + FFmpeg ~100MB = ~400MB total (aman untuk 8GB RAM kamu)

---

*Dibuat dengan ❤️ dan 🇮🇹 Italian Brainrot energy*
