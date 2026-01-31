---
layout: post
title: 'Skip Remotion Setup: Generate Videos in Browser with Just a Prompt'
date: 2026-01-31 00:00:00 +0800
categories: [Tutorial, JavaScript, Animation]
tags: [GSAP, SnapDOM, Video Generation, AI, Browser-Based]
excerpt: 'Learn how to generate professional promo videos in your browser with just a single AI prompt - no installation required.'
---

# Skip Remotion Setup: Generate Videos in Browser with Just a Prompt

## The Remotion Skills Hype

If you've been on X lately, you've probably seen the explosion around **Remotion Agent Skills**. Remotion has integrated with Claude Code to let you generate videos programmatically using AI prompts. It's genuinely impressive, you can create motion graphics, animations, and professional videos just by describing what you want.

But here's the thing: even with AI assistance, Remotion still requires:

- Installing Node.js and setting up a local development environment
- Managing a project structure
- Installing the Remotion agent skills extension
- Need a CLI or IDE (Claude Code, Cursor, etc.)
- Understanding React components and Remotion's API

For someone who just wants to quickly generate a promo video for their SaaS product, this felt like overkill. I wanted something **instant, browser-based, and zero-setup**.

## What I Built Instead

I created a **single-prompt** that generates complete, production-ready promo animations with video recording capability and no installation required.

Here's the entire workflow:

1. Open ChatGPT (or Claude with Canvas enabled)
2. Paste my prompt template
3. Add your product description at the end
4. Generate and preview
5. Click "Record Video" in the generated page
6. Download your `.webm` file

That's it. No IDE. No CLI. No local setup. **Just a browser.**

## See It In Action

Here's a promo video I generated for a fictional product called "ChartGen AI" (a data analytics tool):

The entire animation features:

- ✨ Kinetic typography with GSAP animations
- 🎬 7 professionally sequenced scenes
- 📊 Animated UI mockups with realistic details
- 🎨 Smooth transitions and effects
- 📹 One-click video recording at 24/30/60 FPS

**Total time from prompt to video: ~2 minutes**

## How It Works Technically

The generated code uses a proven tech stack:

- **GSAP 3.12.2**: Powers all kinetic typography and scene transitions
- **SnapDOM**: Captures DOM elements frame-by-frame for video recording
- **Tailwind CSS**: Utility-first styling for rapid UI development
- **MediaRecorder API**: Encodes canvas stream to WebM format

The system includes four key manager classes:

1. **ResourcePreloader**: Ensures fonts load before recording
2. **RenderSyncManager**: Synchronizes DOM rendering with frame capture
3. **SnapDOMCaptureOptimizer**: Throttles captures to match FPS
4. **StateResetManager**: Resets all animations between plays

All the technical implementation details are included in the prompt template below, so the LLM can generate production-ready code every time.

## The Prompt Template

Copy this entire prompt and paste it into ChatGPT Canvas or Claude Artifacts. Just fill in your product details at the end:

````markdown
# SaaS Product Promo Animation Generator

Generate a professional kinetic typography promo animation with video recording capability using GSAP, SnapDOM, and Tailwind CSS.

## ⚠️ Critical Layout Constraints

**Video container dimensions: 854px × 480px**

You MUST ensure all content fits within these bounds:

### Text Content Rules

- ❌ **NEVER** use text that's too long for one line
- ✅ Break long text into multiple lines with `<br>` tags
- ✅ Use shorter, punchier phrases (3-5 words per line max)
- ✅ For longer content, use typewriter effects or staggered reveals
- ✅ Consider viewport zoom for detailed sections (see Demo Scene example)

### UI Mockup Rules

- Maximum width: **700px** (leaves margins)
- Maximum height: **400px** for main content area
- Use **compact** layouts with proper spacing
- Test that all elements are visible without scrolling

### Font Size Guidelines

- Large text: **50-100px** (use sparingly, only for 1-2 word statements)
- Medium text: **35-65px** (for 3-5 word phrases)
- Normal text: **40-80px** (for single words)
- UI text: **11-14px** (for mockup interfaces)

### Overflow Prevention

```javascript
// Example: Long text with line breaks

<div class="kinetic-text medium">
  Spending too much<br>
  on <span class="highlight-red">BAD ADS?</span>
</div>

// Example: Viewport zoom for detailed UI
.call(() => {
const { scale, tx, ty } = calculatePrecisionZoom(
  videoContainer,
  sceneContainer,
  document.getElementById('detailedSection'),
  { desiredScale: 2.0, alignY: 'center' }
);
gsap.to(sceneContainer, { x: tx, y: ty, scale: scale, duration: 1.2 });
}, null, "+=0.2")
```
````

## Technical Stack

- **GSAP 3.12.2**: Animation engine for kinetic typography and transitions
- **SnapDOM**: DOM capture for video recording
- **Tailwind CSS**: Utility-first styling
- **MediaRecorder API**: Video encoding and export

## Core Architecture

### 1. Resource Management System

```javascript
class ResourcePreloader {
  constructor() {
    this.fontsReady = false;
  }
  async preloadFonts() {
    await document.fonts.ready;
    const fontVariants = [
      '400 12px Inter',
      '600 14px Inter',
      '700 14px Inter',
      '900 80px Inter',
    ];
    await Promise.all(fontVariants.map((font) => document.fonts.load(font)));
    this.fontsReady = true;
  }
  async preloadAll() {
    await this.preloadFonts();
  }
  isReady() {
    return this.fontsReady;
  }
}
```

### 2. Render Synchronization

```javascript
class RenderSyncManager {
  static forceLayout(element) {
    void element.offsetHeight;
    void element.offsetWidth;
  }
  static async waitForFrame() {
    return new Promise((resolve) => requestAnimationFrame(resolve));
  }
  static async syncBeforeCapture(element) {
    this.forceLayout(element);
    await this.waitForFrame();
  }
}
```

### 3. SnapDOM Capture System

```javascript
class SnapDOMCaptureOptimizer {
  constructor(targetElement, canvas) {
    this.targetElement = targetElement;
    this.canvas = canvas;
    this.ctx = canvas.getContext('2d', { alpha: false });
    this.lastCaptureTime = 0;
    this.capturePromise = Promise.resolve();
  }

  async capture() {
    await RenderSyncManager.syncBeforeCapture(this.targetElement);
    const capturedCanvas = await snapdom.toCanvas(this.targetElement, {
      width: 854,
      height: 480,
      scale: 1,
      embedFonts: true,
      cacheBust: false,
    });
    this.ctx.clearRect(0, 0, 854, 480);
    this.ctx.drawImage(capturedCanvas, 0, 0, 854, 480);
  }

  async captureWithThrottle(fps) {
    const frameInterval = 1000 / fps;
    const currentTime = performance.now();
    if (currentTime - this.lastCaptureTime >= frameInterval) {
      this.capturePromise = this.capturePromise.then(() => this.capture());
      this.lastCaptureTime = currentTime;
    }
  }
}
```

### 4. State Reset Manager

```javascript
class StateResetManager {
  static resetAllStates() {
    gsap.killTweensOf('\*');
    gsap.set('.scene', {
      opacity: 0,
      x: 0,
      y: 0,
      scale: 1,
      rotation: 0,
      rotationX: 0,
      filter: 'none',
      skewX: 0,
      clearProps: 'all',
    });
    gsap.set(sceneContainer, { scale: 1, x: 0, y: 0, clearProps: 'all' });
    gsap.set(videoContainer, {
      backgroundColor: '#000',
      clearProps: 'backgroundColor',
    });
    // Reset all animated elements
  }
}
```

### 5. Recording System

```javascript
async function startRecording() {
  if (!preloader.isReady()) {
    await preloader.preloadAll();
  }

  const stream = recordCanvas.captureStream(selectedFPS);
  const mimeType = MediaRecorder.isTypeSupported('video/webm;codecs=vp9')
    ? 'video/webm;codecs=vp9'
    : 'video/webm';
  const bitrate = Math.min(selectedFPS \* 100000, 10000000);

  mediaRecorder = new MediaRecorder(stream, {
    mimeType: mimeType,
    videoBitsPerSecond: bitrate
  });

  mediaRecorder.ondataavailable = (e) => {
    if (e.data.size > 0) recordedChunks.push(e.data);
  };

  mediaRecorder.onstop = () => {
    const videoBlob = new Blob(recordedChunks, { type: 'video/webm' });
    // Auto download
    const url = URL.createObjectURL(videoBlob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `promo-${selectedFPS}fps-${Date.now()}.webm`;
    a.click();
    URL.revokeObjectURL(url);
  };

  mediaRecorder.start(100);
  startSyncCapture();
  playAnimation(true);
}
```

## Animation Structure

### Scene Flow Pattern

1. **Hook Scene**: Grab attention with pain point (2-3 seconds)
2. **Problem Scene**: Amplify the problem (2 seconds)
3. **Pain Scene**: Emotional impact with flash effect (2 seconds)
4. **Solution Scene**: Introduce your product (2 seconds)
5. **Feature Scene**: Show key features/benefits (3 seconds)
6. **Demo Scene**: Product UI mockup with animations (5 seconds)
7. **CTA Scene**: Call-to-action with branding (2 seconds)

### GSAP Timeline Structure

```javascript
masterTimeline = gsap.timeline({
  onUpdate: function() {
    if (isRecording) {
      progressFill.style.width = (this.progress() \* 100) + '%';
    }
  },
  onComplete: () => {
    if (isRecording) {
      captureOptimizer.capturePromise.then(() => {
        setTimeout(stopRecording, 500);
      });
    }
  }
});

// Scene transitions
masterTimeline
  .to("#scene1", { opacity: 1, duration: 0.05 })
  .from("#scene1 .text", { scale: 0.5, y: 80, filter: "blur(15px)", duration: 0.35, ease: "back.out(1.7)" })
  .to("#scene1", { scale: 1.1, duration: 1.0 })
  .to("#scene1", { x: -800, skewX: 15, filter: "blur(20px)", opacity: 0, duration: 0.35, ease: "power2.in" });
```

## 🎨 UI Mockup Requirements

### Make It Look REAL

Your UI mockups should look like **actual, production-ready SaaS products**, not wireframes:

#### ✅ DO Include:

- **Realistic data**: Real numbers, percentages, chart values (not "Lorem ipsum")
- **Proper navigation**: Sidebar with icons, top bar with user avatar/settings
- **Visual hierarchy**: Headers, subheaders, labels, descriptions
- **Interactive elements**: Buttons, dropdowns, tabs, toggles (styled properly)
- **Data visualizations**: Charts, graphs, progress bars with actual values
- **Status indicators**: Success/warning/error states, badges, notifications
- **Micro-interactions**: Hover states, active states (use GSAP to animate)
- **Spacing & padding**: Professional whitespace, not cramped
- **Color coding**: Use semantic colors (green for success, red for alerts, etc.)
- **Icons**: Use emojis or Unicode symbols (📊 📈 ⚙️ 🔔 👤)

#### ❌ DON'T:

- Use placeholder text like "Lorem ipsum" or "Sample data"
- Create empty boxes with just borders
- Use generic "Button 1", "Button 2" labels
- Make it look like a wireframe or sketch
- Forget shadows, gradients, and depth

#### Example: Dashboard Mockup

```html
<!-- BAD: Looks fake -->
<div class="box">
  <div>Title</div>
  <div>Content here</div>
</div>

<!-- GOOD: Looks real -->
<div class="bg-slate-800/50 p-4 rounded-lg border border-slate-700 shadow-lg">
  <div class="flex items-center justify-between mb-3">
    <div class="flex items-center gap-2">
      <span class="text-lg">📊</span>
      <h3 class="text-white font-semibold text-sm">Revenue Analytics</h3>
    </div>
    <span class="px-2 py-1 bg-green-500/20 text-green-400 text-xs rounded-full"
      >+12.5%</span
    >
  </div>
  <div class="text-white text-3xl font-bold mb-1">$47,582</div>
  <div class="text-slate-400 text-xs">vs. $42,340 last month</div>
  <div class="mt-3 h-16 flex items-end gap-1">
    <!-- Animated bar chart -->
    <div class="flex-1 bg-purple-500/30 rounded-t" style="height: 45%"></div>
    <div class="flex-1 bg-purple-500/40 rounded-t" style="height: 70%"></div>
    <div class="flex-1 bg-purple-500/50 rounded-t" style="height: 55%"></div>
    <div class="flex-1 bg-purple-500/60 rounded-t" style="height: 85%"></div>
  </div>
</div>
```

### UI Animation Ideas

- **Counter animations**: Numbers counting up from 0
- **Chart bars**: Growing from bottom to top with stagger
- **Progress rings**: Circular progress with SVG animation
- **List items**: Staggered fade-in from left
- **Cards**: Scale up with bounce effect
- **Notifications**: Slide in from top-right
- **Tabs**: Smooth underline transition

## HTML Template Structure

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>[Product Name] - Promo Animation</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/@zumer/snapdom/dist/snapdom.js"></script>

    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      rel="preload"
      href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;900&display=swap"
      as="style"
    />
    <link
      href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;900&display=swap"
      rel="stylesheet"
    />

    <style>
      /* Base styles */
      * {
        box-sizing: border-box;
      }
      body {
        margin: 0;
        padding: 0;
        background: #0a0a0f;
        font-family: 'Inter', sans-serif;
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 100vh;
        flex-direction: column;
        gap: 20px;
      }

      .video-container {
        position: relative;
        width: 854px;
        height: 480px;
        background: #000;
        border-radius: 12px;
        overflow: hidden;
        box-shadow: 0 0 60px rgba(59, 130, 246, 0.15);
        transform: translateZ(0);
        will-change: transform;
      }

      .scene-container {
        position: absolute;
        inset: 0;
        transform-origin: 0 0;
      }

      .scene {
        position: absolute;
        inset: 0;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        opacity: 0;
        pointer-events: none;
      }

      .kinetic-text {
        font-size: clamp(40px, 10vw, 80px);
        font-weight: 900;
        text-transform: uppercase;
        line-height: 1.1;
        text-align: center;
        color: #fff;
        white-space: nowrap;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-rendering: optimizeLegibility;
      }

      .kinetic-text.large {
        font-size: clamp(50px, 12vw, 100px);
      }
      .kinetic-text.medium {
        font-size: clamp(35px, 8vw, 65px);
      }

      /* Color highlights - customize per brand */
      .highlight-red {
        color: #ef4444;
      }
      .highlight-blue {
        color: #3b82f6;
      }
      .highlight-purple {
        color: #a855f7;
      }
      .highlight-green {
        color: #10b981;
      }

      /* Control panel styles */
      .control-panel {
        background: rgba(30, 30, 36, 0.95);
        border: 1px solid rgba(139, 92, 246, 0.3);
        border-radius: 12px;
        padding: 20px;
        width: 854px;
        box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5);
      }

      /* Hidden canvas for recording */
      #recordCanvas {
        position: fixed;
        top: -9999px;
        left: -9999px;
      }
    </style>
  </head>
  <body>
    <!-- Control Panel -->
    <div class="control-panel">
      <div class="flex items-center justify-between">
        <div class="flex items-center gap-4">
          <button id="btnRecord" class="control-btn btn-record">
            <span>🎥</span><span>Record Video</span>
          </button>
          <button id="btnPlay" class="control-btn btn-play">
            <span>▶️</span><span>Play Animation</span>
          </button>
        </div>
        <div class="flex items-center gap-3">
          <div class="library-badge"><span>⚡</span><span>SnapDOM</span></div>
          <div id="statusIndicator" class="status-indicator status-idle">
            <div class="recording-dot"></div>
            <span>Ready</span>
          </div>
        </div>
      </div>

      <div class="fps-selector">
        <span class="fps-label">Frame Rate:</span>
        <div class="fps-option" data-fps="24">24 FPS</div>
        <div class="fps-option" data-fps="30">30 FPS</div>
        <div class="fps-option active" data-fps="60">60 FPS</div>
      </div>

      <div class="progress-bar" id="progressBar" style="display: none;">
        <div class="progress-fill" id="progressFill"></div>
      </div>
    </div>

    <!-- Animation Container -->
    <div class="video-container" id="videoContainer">
      <div class="bg-grid"></div>
      <div class="scene-container" id="sceneContainer">
        <!-- Scene 1: Hook -->
        <div class="scene" id="scene1">
          <div class="kinetic-text medium">
            [Hook Text]<br /><span class="highlight-red">[Pain Point]</span>
          </div>
        </div>

        <!-- Scene 2: Problem -->
        <div class="scene" id="scene2">
          <div class="kinetic-text large">
            [Problem] <span class="highlight-red">[Emphasis]</span>
          </div>
        </div>

        <!-- Scene 3: Pain -->
        <div class="scene" id="scene3">
          <div class="kinetic-text">
            [Action] <span class="highlight-red">[Negative]</span>
          </div>
        </div>

        <!-- Scene 4: Solution -->
        <div class="scene" id="scene4">
          <div class="kinetic-text medium">
            [Action]<br /><span class="highlight-purple">[Solution]</span>
          </div>
        </div>

        <!-- Scene 5: Features -->
        <div class="scene" id="scene5">
          <!-- Feature visualization with icons/animations -->
        </div>

        <!-- Scene 6: Demo (REALISTIC UI mockup) -->
        <div class="scene" id="scene6">
          <!-- Production-quality SaaS interface mockup -->
          <!-- Include: navigation, data, charts, buttons, etc. -->
        </div>

        <!-- Scene 7: CTA -->
        <div class="scene" id="scene7">
          <div class="flex flex-col items-center gap-6">
            <div
              class="cta-logo w-24 h-24 bg-gradient-to-br from-purple-600 to-blue-600 rounded-2xl flex items-center justify-center shadow-lg"
            >
              <span class="text-5xl">[Icon]</span>
            </div>
            <div class="text-center">
              <h1
                class="cta-title text-5xl font-black text-white mb-2 tracking-tight"
              >
                [Product Name]
              </h1>
              <p class="cta-subtitle text-slate-400 text-xl">[Tagline]</p>
            </div>
            <div
              class="bg-gradient-to-r from-purple-600 to-blue-600 text-white px-8 py-3 rounded-full font-bold text-lg hover:scale-105 transition-transform cursor-pointer"
              style="white-space: nowrap;"
            >
              [CTA Button Text]
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Hidden Canvas -->
    <canvas id="recordCanvas" width="854" height="480"></canvas>

    <script>
      // Include all manager classes and animation logic here
      // (ResourcePreloader, RenderSyncManager, SnapDOMCaptureOptimizer, StateResetManager)

      // Configuration
      const ANIMATION_CONFIG = {
        product: { name: '[Product Name]', url: '[Product URL]' },
        timing: { fast: 0.35, hold: 1.0, flashDelay: 0.5, totalDuration: 20 },
        colors: {
          primary: '#8b5cf6',
          danger: '#ef4444',
          success: '#10b981',
          bg: '#0a0a0f',
        },
      };

      // Animation timeline construction
      function playAnimation(autoRecord = false) {
        // Build GSAP timeline with all scenes
      }

      // Recording functions
      async function startRecording() {
        /* ... */
      }
      function stopRecording() {
        /* ... */
      }

      // Event listeners
      btnRecord.addEventListener('click', startRecording);
      btnPlay.addEventListener('click', () => playAnimation(false));

      // Initialization
      resetScene();
      window.addEventListener('load', async () => {
        await preloader.preloadAll();
      });
    </script>
  </body>
</html>
```

## Design Guidelines

### Typography Hierarchy

- **Large**: 50-100px - Single impactful words only
- **Medium**: 35-65px - Short phrases (3-5 words)
- **Normal**: 40-80px - 1-2 words

### Animation Timing

- **Fast**: 0.35s - Quick transitions
- **Hold**: 1.0s - Display duration
- **Flash**: 0.5s - Impact moments

### Color Palette

- **Primary**: Brand color (e.g., #8b5cf6 purple)
- **Danger**: Red (#ef4444) for pain points
- **Success**: Green (#10b981) for positive outcomes
- **Accent**: Blue (#3b82f6) for highlights

### Scene Transitions

1. **Slide + Skew**: Dynamic exit
2. **Scale + Blur**: Dramatic entrance
3. **Rotation**: Playful transitions
4. **Flash**: Emotional impact

## Key Features to Implement

1. ✅ **Auto-download** after recording completes
2. ✅ **FPS selection** (24/30/60)
3. ✅ **Progress indicator** during recording
4. ✅ **Font preloading** for consistent rendering
5. ✅ **State reset** between plays
6. ✅ **Keyboard shortcut** (Space to play)
7. ✅ **Responsive status** indicators

## Requirements

- Video dimensions: **854x480** (480p)
- Font: **Inter** (Google Fonts)
- Total duration: **~20 seconds**
- Export format: **WebM (VP9 codec)**
- Frame rates: **24/30/60 FPS**

## Instructions

Based on the product description below, generate a complete HTML file following this architecture. Create 5-7 scenes that tell a compelling story:

1. **Hook**: Grab attention with a relatable pain point (keep text SHORT)
2. **Problem**: Amplify the problem (1-2 words + emphasis)
3. **Pain**: Create emotional impact (use line breaks for longer text)
4. **Solution**: Introduce the product (punchy tagline)
5. **Features**: Show 1-2 key features with icons/visuals
6. **Demo**: REALISTIC product mockup with rich UI details
7. **CTA**: Strong call-to-action with branding

Customize:

- Scene text content (keep concise!)
- Color scheme (match brand)
- Animations (keep kinetic typography style)
- Demo mockup (make it look production-ready!)
- Icons/emojis (brand-appropriate)

**Remember**: All content MUST fit within 854×480px. Test text length and use line breaks liberally!

---

## Product Description

[YOUR PRODUCT DESCRIPTION GOES HERE]

```

## Try It Yourself

1. Copy the entire prompt above
2. Open ChatGPT (GPT-4 with Canvas) or Claude (with Artifacts enabled)
3. Paste the prompt
4. Fill in your product details at the bottom
5. Wait ~30 seconds for generation
6. Click "Record Video"
7. Download your promo!

## What's Next?

I'm planning to add:

- 🎵 Background music integration
- 🎙️ AI voiceover support
- 📱 Vertical video format (9:16 for social media)
- 🎨 More animation presets
- 🌐 Multi-language support

If you try this out, I'd love to see what you create! Drop a link in the comments.

---

**Found this useful?** Share this post with a founder friend who needs a quick promo video for their next launch.
```
