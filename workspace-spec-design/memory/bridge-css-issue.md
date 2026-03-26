# ISSUE: Bridge CSS Missing (Nightly 2026-03-24)

**Problem:**
- No `/root/bridge-dev/style.css` found during nightly audit.
- Bridge frontend relies on Tactyl CSS variables for GPU-optimized animations and dark theme.

**Expected Vars:**
```css
:root {
  --bg: #0d1117;
  --surface: #161b22;
  --border: #30363d;
  --text: #e6edf3;
  --text-dim: #8b949e;
  --gray: #484f58;
  --green: #3fb950;
  --yellow: #d29922;
  --red: #f85149;
  --blue: #58a6ff;
}
```

**Impact:**
- UI may use hardcoded values → violates Tactyl principles.
- Animations may not be GPU-optimized.

**Action:**
- Create `/root/bridge-dev/style.css` with the above vars.
- Restart OpenClaw Bridge (`systemctl restart openclaw-bridge-dev`).

**Owner:** Captain (`main`).
**Intent:** Resilient [I04], Efficient [I06].