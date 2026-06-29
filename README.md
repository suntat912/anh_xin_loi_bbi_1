# 💛 Gửi Linh Yêu — Trang Xin Lỗi Của Nam Anh

> *"Anh xin lỗi vì đã không trung thực. Anh cảm ơn em — vì dù giận, em vẫn ở đây."*

---

## 📋 Prompt cho Codex

Paste toàn bộ phần dưới đây vào Codex:

---

Build a heartfelt, romantic single-page web app as a personal apology letter from a boyfriend named Nam Anh to his girlfriend Linh. Must look stunning on both mobile phones and laptops.

**Output:** Single file `index.html` — pure HTML + CSS + JS, no frameworks, deployable to GitHub Pages.

**The story behind this page:**
Nam Anh (an IT guy) lied to Linh so he could go drink beer with friends. Linh went silent for a full week — seen but no reply. During that same week, Nam Anh was sick, grinding through work, and pulling all-nighters to finish his graduation project deadline. He was exhausted but messaged her every single day to apologize. Today, he finally finished his thesis defense, poured his heart out, and in a flustered panic even offered to hand over his Facebook account, passwords, and salary card. Linh smiled and finally responded. This page is his follow-through — proof he means it.

**Tone:** Warm, sincere, a little self-deprecating and funny. Like a guy who codes for a living trying his best to be romantic.

---

## 🎨 Design System

**Color palette:**
```
--color-bg:        #FFF5F7   /* nền hồng trắng cực nhạt */
--color-primary:   #FF8FAB   /* hồng chính */
--color-accent:    #FFB3C6   /* hồng nhạt hơn cho card */
--color-gold:      #FFD166   /* vàng ấm cho highlight */
--color-text:      #3D2C35   /* nâu tím đậm, dễ đọc */
--color-soft:      #FFF0F3   /* nền card */
```

**Fonts (Google Fonts):**
- `Dancing Script` — cho tiêu đề lãng mạn, chữ viết tay
- `Nunito` — cho body text, dễ đọc trên mobile

**Breakpoints:**
- Mobile: max-width 480px — font lớn hơn, padding thoải mái, button to dễ bấm
- Tablet: 481px–768px
- Laptop: 769px trở lên — layout có thể rộng hơn, max-width 720px căn giữa

---

## ✨ Chi tiết Animation & Hiệu ứng

### 1. 🌸 Màn hình mở đầu — Hero Section

**Hiệu ứng cánh hoa rơi (Falling Petals):**
- Tạo 20–30 phần tử `<div class="petal">` bằng JS, inject vào body
- Mỗi cánh hoa là một emoji 🌸 hoặc 🌷 hoặc ❤️, kích thước random từ 14px–28px
- Dùng `@keyframes fall` để rơi từ trên xuống dưới màn hình:
  ```css
  @keyframes fall {
    0%   { transform: translateY(-60px) rotate(0deg);   opacity: 1; }
    100% { transform: translateY(110vh) rotate(360deg); opacity: 0; }
  }
  ```
- Mỗi cánh hoa có `animation-duration` random từ 4s–9s và `animation-delay` random từ 0s–6s
- `animation-iteration-count: infinite` — rơi mãi suốt trang
- `left` position random từ 0%–100% để trải đều màn hình
- `pointer-events: none` — không chặn thao tác của người dùng

**Tiêu đề fade-in + slide up:**
- Chữ *"Gửi Linh yêu —"* font Dancing Script, size 52px (mobile: 36px)
- Dùng keyframes:
  ```css
  @keyframes fadeSlideUp {
    0%   { opacity: 0; transform: translateY(30px); }
    100% { opacity: 1; transform: translateY(0); }
  }
  ```
- `animation-duration: 1.2s`, `animation-timing-function: ease-out`
- Subtitle *"Một lời xin lỗi muộn màng — nhưng thật lòng"* xuất hiện sau 0.6s delay

**Animated scroll indicator:**
- Một mũi tên `↓` nhỏ bounce nhẹ ở cuối hero section, gợi ý scroll xuống
  ```css
  @keyframes bounce {
    0%, 100% { transform: translateY(0); }
    50%       { transform: translateY(8px); }
  }
  ```
- `animation-duration: 1.4s`, `animation-iteration-count: infinite`
- Tự ẩn đi sau khi user scroll qua 200px (JS: `window.addEventListener('scroll', ...)`)

---

### 2. 📜 Timeline — Câu chuyện cuộn dần

**Scroll-triggered fade-in (Intersection Observer):**
- Mỗi card timeline ban đầu có `opacity: 0` và `transform: translateY(40px)`
- Dùng `IntersectionObserver` với `threshold: 0.2` — khi card vào 20% viewport thì kích hoạt:
  ```js
  observer.observe(card); // mỗi .timeline-card đều được observe
  ```
- Khi triggered, thêm class `.visible`:
  ```css
  .timeline-card.visible {
    opacity: 1;
    transform: translateY(0);
    transition: opacity 0.6s ease, transform 0.6s ease;
  }
  ```
- Mỗi card có `transition-delay` tăng dần 0.1s để hiệu ứng stagger — không xuất hiện cùng lúc

**Đường line nối timeline:**
- Một đường dọc màu hồng nhạt chạy giữa các card
- Dùng `::before` pseudo-element của container, `height: 100%`, `width: 2px`, `background: linear-gradient(to bottom, #FF8FAB, #FFD166)`
- Mỗi icon emoji ở đầu card có `background: white`, `border: 2px solid #FF8FAB`, `border-radius: 50%`, kích thước 44px — nổi lên trên đường line

**Card hover effect (laptop):**
```css
.timeline-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 32px rgba(255, 143, 171, 0.25);
  transition: all 0.3s ease;
}
```
- Trên mobile: không có hover, thay bằng `active` state khi tap — card scale nhẹ `scale(0.98)`

---

### 3. 💌 Apology Block — Lời xin lỗi chính

**Hiệu ứng typewriter (gõ chữ từng ký tự):**
- Đoạn text xin lỗi chính được gõ ra từng chữ một khi section này scroll vào viewport
- Dùng JS setInterval, mỗi 35ms thêm một ký tự vào innerHTML
- Con trỏ nháy `|` xuất hiện ở cuối trong lúc đang gõ:
  ```css
  @keyframes blink {
    0%, 100% { opacity: 1; }
    50%       { opacity: 0; }
  }
  .cursor { animation: blink 0.8s infinite; }
  ```
- Sau khi gõ xong, cursor tự ẩn sau 2s

**Background card pulse nhẹ:**
- Card xin lỗi có `box-shadow` animate nhẹ liên tục:
  ```css
  @keyframes softPulse {
    0%, 100% { box-shadow: 0 4px 24px rgba(255, 143, 171, 0.15); }
    50%       { box-shadow: 0 4px 36px rgba(255, 143, 171, 0.35); }
  }
  animation: softPulse 3s ease-in-out infinite;
  ```

---

### 4. 💛 Nút "Tha thứ cho anh không?"

**Nút glow + ripple:**
- Nút *"Có 💛"* có hiệu ứng glow liên tục:
  ```css
  @keyframes glowPulse {
    0%, 100% { box-shadow: 0 0 12px #FFD166, 0 0 24px #FFB3C6; }
    50%       { box-shadow: 0 0 24px #FFD166, 0 0 48px #FF8FAB; }
  }
  animation: glowPulse 2s ease-in-out infinite;
  ```
- Khi bấm: hiệu ứng ripple toả ra từ điểm chạm:
  ```js
  // Tạo div.ripple tại vị trí click/touch, scale từ 0 lên 300%, rồi fade out và xoá
  ```

**Khi bấm "Có 💛" — Confetti explosion:**
- Dùng `canvas-confetti` (CDN): bắn confetti 3 lần liên tiếp với delay 200ms
- Lần 1: bắn từ góc trái (`origin: { x: 0, y: 0.8 }`)
- Lần 2: bắn từ giữa (`origin: { x: 0.5, y: 0.6 }`)
- Lần 3: bắn từ góc phải (`origin: { x: 1, y: 0.8 }`)
- Màu confetti: `['#FF8FAB', '#FFD166', '#FFB3C6', '#ffffff', '#FF4D6D']`
- Sau confetti: text *"Cảm ơn em 🥺"* fade-in ở giữa màn hình, font Dancing Script 48px, màu hồng đậm

**Khi bấm "Chưa 😤" — Wiggle animation:**
```css
@keyframes wiggle {
  0%,100% { transform: rotate(0deg); }
  20%      { transform: rotate(-6deg); }
  40%      { transform: rotate(6deg); }
  60%      { transform: rotate(-4deg); }
  80%      { transform: rotate(4deg); }
}
```
- Nút lắc 0.5s rồi dừng
- Text dưới nút đổi thành *"Anh hiểu... anh sẽ tiếp tục cố 🙏"* với fade-in nhẹ
- Nút *"Chưa 😤"* tự disable sau 3 lần bấm, đổi text thành *"Anh vẫn chờ em 💛"*

---

### 5. ❤️ Closing Section — Lời kết

**Beating heart animation:**
```css
@keyframes heartbeat {
  0%, 100% { transform: scale(1); }
  14%       { transform: scale(1.2); }
  28%       { transform: scale(1); }
  42%       { transform: scale(1.15); }
  70%       { transform: scale(1); }
}
```
- Một trái tim ❤️ lớn (72px) đập nhịp nhàng, 2 lần đập rồi nghỉ, lặp lại mỗi 1.5s — giống nhịp tim thật
- Chữ *"Yêu em — Nam Anh 🧑‍💻"* fade-in bên dưới sau 0.8s

**Particle trail theo chuột (laptop only):**
- Trên desktop: khi di chuột qua closing section, tạo ra các chấm nhỏ ✨ màu hồng-vàng xuất hiện theo vị trí chuột rồi fade out sau 600ms
- Dùng JS `mousemove` event, tạo element, append vào body, xoá sau khi animation xong
- Không áp dụng trên mobile (check `window.matchMedia('(pointer: coarse)')`)

---

## 📱 Responsive Chi Tiết

| Thành phần | Mobile (≤480px) | Laptop (≥769px) |
|---|---|---|
| Hero title | 36px | 52px |
| Cánh hoa rơi | 15 cánh | 28 cánh |
| Timeline card | Full width, padding 16px | Max-width 600px, căn giữa |
| Nút "Có / Chưa" | Width 100%, height 56px | Width 200px, height 52px |
| Hover effects | Tắt (dùng active thay thế) | Bật đầy đủ |
| Particle trail | Tắt | Bật |
| Confetti lượng | 80 particles | 200 particles |

---

## 🛠️ External Dependencies (CDN only)

```html
<!-- Google Fonts -->
<link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@600;700&family=Nunito:wght@400;600;700&display=swap" rel="stylesheet">

<!-- Confetti -->
<script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.9.2/dist/confetti.browser.min.js"></script>
```

---

## 🚀 Deploy lên GitHub Pages

### Bước 1 — Tạo repo GitHub
Vào [github.com/new](https://github.com/new), đặt tên: `xin-loi-linh`
> ⚠️ Để **Public** thì Linh mới xem được qua link

### Bước 2 — Push code
```bash
git init
git add index.html README.md
git commit -m "💛 anh xin lỗi em"
git branch -M main
git remote add origin https://github.com/your-username/xin-loi-linh.git
git push -u origin main
```

### Bước 3 — Bật GitHub Pages
```
Settings → Pages → Source: main → / (root) → Save
```

### Bước 4 — Gửi cho Linh
```
https://your-username.github.io/xin-loi-linh/
```

---

## 💡 Gợi ý lời nhắn khi gửi link

> *"Em ơi, anh là dân IT nên cách xin lỗi của anh hơi khác một chút. Anh làm cái này cho em: [link] 💛"*

---

*Chúc Nam Anh và Linh mãi bên nhau 🥺💛*
