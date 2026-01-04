<img src="screenshots/stm-logo-cropped.png" alt="Stranger Things Melody" width="50%" />

Recreate the iconic Stranger Things theme melody note-by-note with this interactive Vue.js application powered by Web Audio API.

[![Vue.js](https://img.shields.io/badge/Vue.js-4FC08D?style=flat&logo=vue.js&logoColor=white)](https://vuejs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=flat&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/CSS)
[![Vite](https://img.shields.io/badge/Vite-646CFF?style=flat&logo=vite&logoColor=white)](https://vitejs.dev/)
[![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat&logo=node.js&logoColor=white)](https://nodejs.org/)
[![NPM](https://img.shields.io/badge/NPM-CB3837?style=flat&logo=npm&logoColor=white)](https://www.npmjs.com/)
[![Web Audio API](https://img.shields.io/badge/Web_Audio_API-FF6B35?style=flat&logo=audiomack&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)

## Table of Contents
1. [Learning Goals](#learning-goals)
2. [Prerequisites](#prerequisites)
3. [Project Setup](#project-setup)
4. [Design Decisions](#design-decisions)
5. [Initial Audio Testing](#initial-audio-testing)
6. [Multiple Notes](#multiple-notes)
7. [Keyboard Functionality](#keyboard-functionality)
8. [Rolling Keys](#rolling-keys)
9. [More Design Decisions](#more-design-decisions)
10. [Future Enhancements](#future-enhancements)
11. [Contact](#contact)

## Learning Goals

This project served as my introduction to Vue.js, focusing on:

- Reactive references
- Composition API state management
- Lifecycle hooks
- Web Audio API integration for real-time sound generation

## Prerequisites

Before you begin, ensure you have the following installed:

### **Node.js**
- Download from: https://nodejs.org/
- Verify installation:
```bash
node -v
```

### **NPM**
- Comes with Node.js
- Verify installation:
```bash
npm -v
```

### **Git** (optional, for cloning)
- Download from: https://git-scm.com/
- Verify installation:
```bash
git --version
```

---

## Project Setup

### 1. Clone the Repository

```bash
git clone https://github.com/SeaForeEx/stranger-things-melody.git
cd stranger-things-melody
```

### 2. Install Dependencies
```bash
npm install
```

### 3. Run the Development Server
```bash
npm run dev
```

Open your browser to `http://localhost:5173/`

---

## Design Decisions

One thing I've learned about my process is that I prefer making the site visually appealing before diving into functionality. For some reason, working on code becomes easier when I genuinely like what I'm looking at.

### Desktop View

<img src="screenshots/stm-desktop.png" alt="STM Desktop View" width="75%" />

I downloaded a free Stranger Things-style font from [FontGet](https://www.fontget.com/font/stranger-things-1/). It included an outlined version that I used in the "Welcome to Hawkins" header.

Then I generated the Stranger Things Melody background using the Pixel Frame [Stranger Things Font Generator](https://pixelframe.design/stranger-things/).

Finally, I created five buttons representing the five notes in the basic Stranger Things melody, with each note labeled on its button. I considered simulating an actual keyboard layout, but chose individual buttons to create a more intuitive experience for users who have never played piano before.

### Mobile View

<img src="screenshots/stm-mobile.png" alt="STM Mobile View" width="50%" />

When designing the mobile view, I realized the desktop background didn't translate well to taller screens. I considered creating a different background entirely, but then it hit me: I could stack the background vertically with the bottom half flipped upside down—mirroring the Upside Down from Stranger Things. It solved the problem creatively and stayed true to the theme. Everyone wins!

---

## Initial Audio Testing



---

## Multiple Notes

---

## Keyboard Functionality

---

## Rolling Keys

---

## More Design Decisions

---

## Future Enhancements

---

## Contact

Questions or feedback? Feel free to reach out:
- **Email:** charlesbridgersiv@gmail.com
- **LinkedIn:** [Charles Bridgers IV](https://www.linkedin.com/in/charlesbridgersiv/)
- **GitHub:** [SeaForeEx](https://github.com/SeaForeEx)

---

Thank you for reviewing my Stranger Things Melody App!
