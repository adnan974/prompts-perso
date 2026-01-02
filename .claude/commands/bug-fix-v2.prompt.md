# 🐛 Bug Fix Assistant

## Context
- **Issue**: [GIVEN BY USER]
- **Expected behavior**: [GIVEN BY USER]

---

## 📋 Menu - Choose your debugging approach:

### **1. Deep Analysis + Hypothesis**
> Thorough code analysis: trace action paths, identify breaking points, provide 3 ranked hypotheses with confidence levels.
> Best for: Understanding the bug before deciding on a fix strategy.

### **2. Analysis + Logging**
> Analyze code paths and inject strategic console.log/debug statements to trace the issue at runtime.
> Best for: Intermittent bugs, race conditions, async issues, unclear reproduction steps.

### **3. Full Investigation**
> Complete workflow: understand → trace → hypothesize → fix → verify.
> Best for: Complex bugs, unfamiliar codebase areas, when you want guided resolution.

**⏸️ STOP HERE - Ask user to select option (1, 2, or 3) before proceeding.**

---

## Workflow (adapts based on selected choice)

### Step 1: Understand
- Summarize the issue in your own words (confirm understanding)
- Identify the feature/module affected
- List user action path (click → function → state → UI)

### Step 2: Investigate
- Find relevant files based on action path
- Check recent changes in those files (if git available)
- Identify potential breaking points

### Step 3: Hypothesize (Options 1 & 3)
- List **3 potential causes** with confidence level

### Step 3b: Inject Logging (Option 2 only)
- Place `console.log` at:
  - Entry points (action dispatched, function called)
  - State mutations (before/after)
  - API calls (request/response)
  - Conditional branches (which path taken)
- Use consistent format: `console.log('[DEBUG][ComponentName] description:', value)`
- Add timestamps for async flows: `console.log('[DEBUG][${Date.now()}] ...')`
- Guide user to reproduce and share console output

### Step 4: ⏸️ CHECKPOINT - Wait for user confirmation

### Step 5: Solution (Option 3 only)
- Provide **fix approaches**. Rank probable causes with evidence-based reasoning.

### Step 6: ⏸️ CHECKPOINT - Wait for user confirmation

### Step 7: Implement & Verify (Option 3 only)
- Apply the fix
- Run related tests
- Suggest additional test cases if needed
