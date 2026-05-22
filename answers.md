# Answers

## 1. How to run

No installs are needed beyond a browser. Double-click `pomodoro.html` to open it directly.

## 2. Stack & design choices

I used vanilla HTML, CSS, and JavaScript because the app is a single screen with no routing, no shared state between components, and no data fetching. A framework would have added extra dependencies and files without giving real benefit.

**Decision 1: the whole UI changes color, not just an indicator.**
When you switch from focus to break, the background, the ring, the glow, and the button all shift from indigo to green together. I did this because a small status badge is easy to miss mid-session. Changing the entire mood of the screen makes it impossible to not notice the transition, which matters when you're deep in work and only glance up briefly.

**Decision 2: the ring depletes rather than fills.**
A fill starts empty and feels like waiting. Instead, a shrinking/depleting ring starts full and shrinks which matches how time actually works, and gives a quiet sense of urgency as the circle gets smaller. In the last 5 minutes of a focus session the color also shifts to warm orange, so you get a visual warning without any text or sound interrupting you.

---

## 3. Responsive & accessibility

On a **360px phone**, the ring scales down to 85% of the viewport width using min(320px, 78vw), the font size uses clamp() so the countdown never overflows, and padding messes up. Everything is in a single column so nothing wraps awkwardly.

On a **1440px laptop**, the layout stays centered with a max-width: 480px container. It does not stretch to fill the screen because a timer is a focused tool and a wide layout would make it feel like a dashboard, which is the not the feeling I was going for.

**Accessibility handled:** All interactive elements like start, reset, skip, settings, and the +/− buttons are real `<button>` elements, so they are can be selected with a keyboard and work with Enter/Space by default. The skip button has a title attribute so users can hover to know its purpose.

**Accessibility skipped:** I didn't cater for screen readers announcing the countdown ticking down every second. It would be noisy and annoying for screen reader users. A better solution would be to announce only key moments (session started, session ended) but that was outside the scope of this project.

---

## 4. AI usage

**What I used it for and what I changed:**

I used Claude to help design the CSS, including the background gradient, color theme transitions, and improving the animations specifically to make them feel both smooth and intentional rather than abrupt. For example the ring glow, the breathing pulse, and the full-UI color shift from focus to break were all refined assistance from AI to get the timing and ease in and ease out curves right.

One thing I changed: the AI originally animated the ring glow by toggling between multiple CSS classes. I simplifed this into a single animation that reads from a CSS variable, so changing the theme automatically updates the glow color without any extra swapping logic. The result is easier to read and easier to extend if you want to add more themes later.

---

## 5. Honest gap

The breathing glow animation on the ring uses CSS `filter: drop-shadow()` which is recalculated by the browser every frame. On low-end devices this can cause the animation to stutter especially when the CSS color transition is also running at the same time.

The fix would be to remove the filter animation from the ring SVG itself and use a separate div and change its opacity. I did not do this because the stutter is not visible on modern devices and fixing it would have added layout complexity which would not keep the code simple.