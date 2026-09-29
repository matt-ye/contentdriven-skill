# contentdriven — Claude skill for operating ContentDriven

> **繁體中文**：讓 Claude 安全地操作 ContentDriven 的 skill。開場先問你要怎麼發（立即、排程或只做草稿；哪些平台；文案逐字或讓 AI 寫；導流去哪），在「生成」前設好導流與留言 CTA，把文案覆寫回你核准的版本、上傳圖片、送審，並到審核台逐篇驗證。**任何會公開發佈的動作都要你在對話裡明確同意**；不碰帳密、不改低審核風險。安裝方式見下方 Install；實測整理的操作提醒在 `references/platform-notes.md`。

A Claude skill that stages social posts on [ContentDriven](https://app.contentdriven.ai) safely: it asks how you want to publish (now, scheduled, or drafts only; which platforms; exact captions or AI-written; where traffic should go), sets everything that must be set **before** generating, restores your approved captions, uploads media, submits for review, and checks the review queue — without ever publishing on its own.

Built from a real 15-post Instagram launch on the platform (2026-09-28/29). The operating tips it follows are in [`references/platform-notes.md`](references/platform-notes.md).

## What it will and won't do

- ✅ Generate drafts, overwrite them with your exact captions, upload images, set the traffic link and comment CTA, submit to review, verify each post in 審核台, schedule through 內容計畫.
- ✅ Batch many posts from a list you give it (captions + image paths).
- ⛔ Publish, approve, schedule or pin **without your explicit OK** in chat.
- ⛔ Bind a low-risk strategy (low risk auto-publishes on 送審).
- ⛔ Type passwords, connect social accounts, authorize OAuth, or buy points — you do those.

## Requirements

- A ContentDriven account with the brand set up and social accounts connected.
- **Claude in Chrome** (the browser extension) signed in, so Claude works in your own logged-in Chrome.
- Claude Code or claude.ai with skills enabled.

## Install

**Claude Code** — copy the folder into your personal skills:

```bash
git clone https://github.com/matt-ye/contentdriven-skill ~/.claude/skills/contentdriven
```

Then start a new session and say "use ContentDriven to …" or type `/contentdriven`.

**claude.ai** — zip the folder so `SKILL.md` sits at the top level of the zip (not inside a subfolder), then upload it under Settings → Capabilities → Skills.

## Files

| File | What it is |
|---|---|
| `SKILL.md` | The skill: interview → pre-flight checks → workflows (post now / schedule / review) → reporting |
| `references/platform-notes.md` | Operating tips from hands-on use, automation notes, points |
| `references/strategy-template.md` | A `review_risk: high` strategy template for 策略庫 |

## Keeping it current

The platform changes. When something doesn't match, trust the screen, then update `references/platform-notes.md` (and the date at its top) in a PR so the next person benefits.

Not tried yet: how 導流 is set for drafts created by 內容計畫, and multi-image carousel upload.
