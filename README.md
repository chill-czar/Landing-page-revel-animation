## **🚀 Teaching You the CSS & JS Part Like a Pro!**  

Now, let’s **deep dive** into how the CSS & GSAP (`script.js`) work together to create **awesome animations**. I'll explain it in a way that helps you **understand and use it in your future projects**.  

---

# **🎨 1️⃣ Understanding `style.css` (Styling & Layout)**  

CSS is responsible for:  
✅ **Structuring elements properly** (positioning, display).  
✅ **Styling elements** (colors, fonts, sizes).  
✅ **Preparing animations** (like `clip-path` for GSAP).  

---

### **🟢 1.1 Setting Up Global Styles**
```css
@font-face {
    font-family: 'CustomFont';
    src: url('./PPNeueMontreal-Regular.79dd52.woff2') format('woff2');
    font-weight: normal;
    font-style: normal;
}
```
✅ **Loads a custom font (`woff2`) from the local folder.**  

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'CustomFont';
}
```
✅ **Resets margins, padding, and applies the font** to all elements.

```css
html, body {
    height: 100%;
    width: 100%;
}
```
✅ **Ensures the full page takes up 100% of the viewport**.

---

### **🟢 1.2 Styling the Hero Section**
```css
.hero {
    position: relative;
    width: 100%;
    height: 100svh;
    background-color: #15151b;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    will-change: clip-path;
}
```
✅ `.hero` is the **main wrapper**:  
- `position: relative;` → Allows absolute-positioned children inside.  
- `display: flex;` → Organizes content inside.  
- `will-change: clip-path;` → **Optimizes performance for GSAP animations.**  

---

### **🟢 1.3 Progress Bar (Loading Bar)**
```css
.progress-bar {
    position: absolute;
    top: 50%;
    left: 0;
    transform: translateY(-50%);
    width: 25vw;
    padding: 2em;
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: #ffbb00;
}
```
✅ **Makes the progress bar visible & centered.**  
- `top: 50%; transform: translateY(-50%);` → Centers vertically.  
- `width: 25vw;` → Makes it 25% of the viewport width.  
- **GSAP increases `width: 100%` dynamically!**  

---

### **🟢 1.4 Background Video Styling**
```css
.video-container {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 100%;
    height: 100svh;
    background-color: #000;
    clip-path: polygon(20% 20%, 80% 20%, 80% 80%, 20% 80%);
    will-change: transform, clip-path;
    overflow: hidden;
}
```
✅ **Centrally positions the video container.**  
✅ **Initial `clip-path` is applied** (GSAP animates this later!).  

```css
.video-container video {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: auto;
    height: auto;
    min-width: 100%;
    min-height: 100%;
    object-fit: cover;
    opacity: 0.85;
}
```
✅ **Ensures the video covers the entire container.**  

---

### **🟢 1.5 Animated Text (Header & Coordinates)**
```css
.header span, .coordinates p span {
    position: relative;
    display: block;
    transform: translateY(100%);
    will-change: transform;
}
```
✅ **Hides text initially (`translateY(100%)`)**, GSAP animates it **upwards (`y: 0%`)**.

---

### **🟢 1.6 Logo Animation Setup**
```css
.logo {
    position: absolute;
    bottom: 0;
    left: 50%;
    display: flex;
    transform: translateX(-50%);
    will-change: transform;
    color: #fff;
    mix-blend-mode: difference;
}
```
✅ **Initially positioned at the bottom & centered horizontally.**  
✅ **GSAP animates `translateX(0%)` to move it in.**  

---

## **🔥 2️⃣ Now, Let’s Dive into `script.js` (GSAP Animations!)**  

Now that the **CSS is ready**, GSAP **animates everything dynamically**.  

---

### **🟢 2.1 Register GSAP Plugins**
```js
gsap.registerPlugin(CustomEase);
```
✅ **Registers GSAP’s `CustomEase` plugin** (needed for smooth animations).  

```js
const customEase = CustomEase.create("custom", ".87,0,.13,1");
```
✅ **Creates a custom easing curve** for smoother animations.  
- `.87,0,.13,1` → Custom Bézier curve for **smooth acceleration & deceleration**.  

---

### **🟢 2.2 Set Initial State**
```js
gsap.set(".video-container", {
  scale: 0,
  rotation: -20,
});
```
✅ **Hides the video (`scale: 0`)** & **tilts it (`rotation: -20`)**.  
✅ **GSAP later animates it back to `scale: 1`, `rotation: 0`**.  

---

### **🟢 2.3 Step 1: Start the Clipping Animation**
```js
gsap.to(".hero", {
  clipPath: "polygon(0% 45%, 25% 45%, 25% 55%, 0% 55%)",
  duration: 1.5,
  ease: customEase,
  delay: 1,
});
```
✅ **Animates the `.hero` section from a small clip to a larger one.**  

---

### **🟢 2.4 Step 2: Expand Hero & Start Progress Bar Animation**
```js
gsap.to(".hero", {
  clipPath: "polygon(0% 45%, 100% 45%, 100% 55%, 0% 55%)",
  duration: 2,
  ease: customEase,
  delay: 3,

  onStart: () => {
    gsap.to(".progress-bar", {
      width: "100%",
      duration: 2,
      ease: customEase,
    });

    gsap.to("#counter", {
      innerHTML: 100,
      duration: 2,
      ease: customEase,
      snap: { innerHTML: 1 },
    });
  },
});
```
✅ **Expands `.hero` fully while increasing progress bar width (`width: 100%`).**  
✅ **Counter updates from `0 → 100` dynamically.**  

---

### **🟢 2.5 Step 3: Reveal Video & Logo**
```js
gsap.to(".hero", {
  clipPath: "polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)",
  duration: 1,
  ease: customEase,
  delay: 5,
  onStart: () => {
    gsap.to(".video-container", {
      scale: 1,
      rotation: 0,
      duration: 1.25,
      ease: customEase,
    });

    gsap.to(".logo", {
      left: "0%",
      transform: "translateX(0%)",
      duration: 1.25,
      ease: customEase,
    });
  },
});
```
✅ **Reveals the video (`scale: 1` & `rotation: 0`).**  
✅ **Moves in the logo (`translateX(0%)`).**  

---

### **🟢 2.6 Step 4: Animate Text & Coordinates**
```js
gsap.to([".header span ", ".coordinates span"], {
  y: "0%",
  duration: 1,
  stagger: 0.125,
  ease: "power3.out",
  delay: 5.75,
});
```
✅ **Moves text into view with staggered animation.**  

---

## **🚀 What You Learned Today**
✔ **How CSS prepares elements for animations**.  
✔ **How GSAP animates them step-by-step**.  
✔ **How `clipPath`, `scale`, and `translateX/Y` create smooth transitions**.  

Would you like to **add more effects** like hover animations or scroll interactions? 🚀
