# Using Garry Tan's gstack with OpenCode

NOTE: I haven't tested this yet. Just spreading the love. Written by Grok.

### (Bypass Claude Code Entirely)

These instructions show how to adapt Garry Tan's **gstack** (specialized engineering team slash commands) to work inside **OpenCode** — the leading open-source terminal AI coding agent that replaces Claude Code.

This setup lets you keep the "team of hats" philosophy (**CEO, Eng Lead, Paranoid Reviewer, Ship, Browse QA, Retro**) while using any model provider — including pay-per-request Anthropic Claude, OpenRouter (free/cheap Claude or alternatives), local Ollama, Gemini, Groq, etc.

* **Date of instructions:** March 2026
* **Tested concepts:** OpenCode v0.9+ / gstack latest

---

## Prerequisites

* **Node.js 18+** (for OpenCode installation)
* **Git** installed
* **A model API key ready:**
* **Anthropic API key** (for best fidelity) → [console.anthropic.com](https://console.anthropic.com)
* **OpenRouter key** → [openrouter.ai/keys](https://openrouter.ai/keys)
* **Local Ollama** running



---

## Step 1: Install OpenCode

Run one of these (latest version recommended):

```bash
# Easiest one-liner (recommended)
curl -fsSL https://opencode.ai/install | bash

```

**Alternative methods:**

```bash
# npm global
npm install -g opencode-ai@latest

# Homebrew (macOS / Linux)
brew install anomalyco/tap/opencode

```

**Verify:**

```bash
opencode --version
opencode          # should launch the TUI

```

*Exit with `Ctrl+C` or type `/exit`.*

---

## Step 2: Configure Your Model

### Option A: Use Real Anthropic Claude (Recommended)

```bash
export ANTHROPIC_API_KEY=sk-ant-api03-XXXXXXXXXXXXXXXXXXXX

```

OpenCode will detect it automatically.

### Option B: OpenRouter (Multi-model)

```bash
export OPENROUTER_API_KEY=sk-or-v1-...

```

In your OpenCode session, select: `qwen/qwen3-coder:free`, `google/gemini-2.5-pro-preview`, or `deepseek/deepseek-r1`.

### Option C: Local (Zero Cost)

Run Ollama first:

```bash
ollama serve
ollama pull qwen2.5-coder:32b

```

Then connect OpenCode to your local Ollama instance.

---

## Step 3: Get gstack Prompts (Reference)

Clone Garry's repo to copy the original prompts:

```bash
git clone https://github.com/garrytan/gstack.git ~/gstack-reference
cd ~/gstack-reference
ls *.md

```

---

## Step 4: Create OpenCode Skills from gstack

OpenCode loads skills from markdown files in these locations:

* **Project:** `.opencode/skills/<skill-name>/SKILL.md`
* **Global:** `~/.config/opencode/skills/<skill-name>/SKILL.md`

### Mapping Table

| gstack Command | OpenCode Skill Folder | Source File | When to Use It |
| --- | --- | --- | --- |
| `/plan-ceo-review` | `ceo-review` | `plan-ceo-review.md` | Big-picture, 10x vision |
| `/plan-eng-review` | `eng-lead-review` | `plan-eng-review.md` | Architecture, data flow |
| `/review` | `paranoid-review` | `review.md` | Deep bug/security review |
| `/ship` | `ship` | `ship.md` | Ready-to-ship, PR steps |
| `/browse` | `browse-qa` | `browse.md` | Visual/UI QA |
| `/retro` | `retro` | `retro.md` | Git history, team metrics |

---

## Step 5: Add a System Prompt

Create or edit `.opencode/opencode.json` in your project:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "systemPrompt": "You are a high-velocity engineering team inspired by Garry Tan's gstack. Use specialized skills via the 'skill' tool when the task matches: ceo-review for founder vision, eng-lead-review for technical architecture, paranoid-review for deep code review & security, ship for PR/release, browse-qa for visual/UI/browser QA, retro for git metrics & retrospectives. Ask which mode to use if unclear. Stay concise, tasteful, and ship fast."
}

```

---

## Step 6: How to Use It

1. `cd` into your project and run `opencode`.
2. Switch to **build agent** (Tab key) for write access.
3. **Ask naturally:**
* *"Review our roadmap as CEO"* (Triggers `ceo-review`)
* *"Use skill paranoid-review on this PR diff"*
* *"Do visual QA on http://localhost:3000 – use browse-qa"*



---

### 💡 Tips & Troubleshooting

* **Skills not loading?** Restart OpenCode and ensure `SKILL.md` is uppercase.
* **Browser tool:** OpenCode’s native browser (Playwright-based) is usually faster than gstack’s custom binary.
* **Update:** Run `opencode upgrade` regularly.
