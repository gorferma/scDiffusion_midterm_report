# Slidev Development Pitfalls & Best Practices

This document summarizes lessons learned from debugging the `scDiffusion-Presentation` project.

## 1. Markdown & Math inside HTML
**The Issue:**
Mathematical expressions (LaTeX) and Markdown formatting (bold, italics) fail to render when wrapped tightly inside HTML tags like `<div>` or `<p>`. The parser treats the content as raw HTML text.

**The Fix:**
Always add **blank lines** separating the opening and closing HTML tags from the content.

❌ **Incorrect:**
```html
<div class="text-center">
  $$ E = mc^2 $$
</div>
```

✅ **Correct:**
```html
<div class="text-center">

$$ E = mc^2 $$

</div>
```

**Note:** For inline math like `$T$`, standard Markdown parsers are picky. Ensuring the surrounding container parses as Markdown (via the blank line method) is usually required.

---

## 2. Animations with `v-click`
**The Issue:**
Standard CSS animation classes (e.g., `animate-bounce-in`) trigger immediately when the slide loads. If an element is hidden via `v-click`, the animation plays invisibly in the background. When you finally click to reveal it, the animation has already finished.

**The Fix:**
Use CSS Transitions combined with Slidev's `v-click` state class.

1. **Add a global style for the hidden state:**
   ```css
   <style>
   .slidev-vclick-hidden {
     transform: translateY(20px);
     opacity: 0;
     pointer-events: none;
   }
   </style>
   ```

2. **Apply transition classes to the element:**
   ```html
   <div v-click class="transition-all duration-500">
     Content
   </div>
   ```

This ensures the browser transitions the element from "hidden" to "visible" dynamically whenever the class changes (forward or backward).

---

## 3. Layout Cutoffs
**The Issue:**
Absolute positioning (e.g., `absolute bottom-10`) or large margins can push content off-screen, especially on different display aspect ratios.

**The Fix:**
*   Be conservative with bottom spacing (prefer `bottom-16` or higher).
*   Use `mt-4` instead of `mt-8` for spacing between elements if the slide is dense.
*   Test slides in full-screen mode frequently.

---

## 4. YAML Frontmatter Config
**The Issue:**
Duplicate keys in the Frontmatter (e.g., defining `transition: slide-left` twice) causes a hard `YAMLParseError` that crashes the dev server.

**The Fix:**
Ensure global configurations (like `transition`, `theme`) appear only once in the primary Frontmatter block.

---

## 5. LaTeX Syntax Strictness
**The Issue:**
A single syntax error in a LaTeX block (like an extra closing brace `}}`) can cause the Markdown parser to fallback to rendering the raw text source code instead of a math block.

**The Fix:**
Isolate complex equations and double-check brace matching. If a block looks like raw text, it is almost always a syntax error or the "HTML wrapper" issue mentioned in #1.
