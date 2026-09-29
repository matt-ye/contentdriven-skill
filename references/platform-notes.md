# ContentDriven — tested behaviour and traps

Tested on app.contentdriven.ai between 2026-09-28 and 2026-09-29 while staging a 15-post Instagram launch (trial plan). The platform changes; if something below no longer matches what you see, trust the screen and update this file.

## How posts move

`命題 → 生成 → 草稿卡 → 送審 → 審核台 → 核可 → published (or scheduled)`

| Step | What is saved | Notes |
|---|---|---|
| 生成 | The draft, **plus the 導流 settings and 留言 CTA as they were in the composer at that moment** | Changing 導流 after generating has no effect anywhere. The only fix is 退回／捨棄 and generating again (text generation is cheap). |
| Editing 發文內容 on the draft card | **Nothing, until 送審** | A page reload throws the edit away. Media uploads are saved immediately. |
| 修改指示 → 送出修改 | An AI rewrite | Not a verbatim save. |
| 送審 | The caption as typed, media, and the saved 導流/CTA | Then the bound strategy decides: "低風險無旗標自動核可並發佈，其餘進審核台". |
| 核可 | Publishes | Drafts made on the 貼文 page carry **no publish time**, so 核可 = publish now. Only drafts from 內容計畫 carry a time; 核可 schedules them for it (or publishes now if that time has passed). |

## Composer (貼文 page)

- **Defaults reset on every page load**: 導流 = LINE 諮詢 on, 自動留言 on, with the Chinese LINE CTA template. For anything else, reset it before every 生成.
- 導流 options: LINE 諮詢連結 (needs a LINE OA and a LINE link), 自訂 URL (any destination; wrapped in a click-counting short link), or both off (impressions only).
- **The generator does not honour "use this exact caption".** Observed: added emoji and punctuation, appended a CTA sentence, and once **invented product parameters** ("N of 24 and L of 6") for a pattern that doesn't exist yet. Always overwrite 發文內容 with the approved text before 送審.
- 留言內容 (CTA) is a single-line input; line breaks collapse.
- 發布後自動留言 is labelled 即將生效 — not live. Nothing is commented automatically; post first comments by hand.
- There is **no alt-text field**. Add alt text in the Instagram app after publishing.
- The platform preview for IG says comment links are plain text (not clickable); the bio link is the real traffic path.
- Media: 自行上傳 accepts PNG/JPG (1.2 MB files were fine). Multi-image carousel upload: untested.
- Generation takes about 50–75 seconds per post.

## Strategy (策略庫)

- A strategy is a Markdown file with frontmatter `vertical` and `review_risk`, and sections 角色／禁詞／內容主題／結構／範例／合規規則. See `strategy-template.md`.
- 禁詞 must be **one per line** — a comma-separated list is parsed as a single word.
- `review_risk: high` sends every submission to 審核台. **Never bind a low-risk strategy for a brand where every post must be seen by a human.**
- The compliance checker flags posts against the strategy's 合規規則. Rules written as "whenever X comes up, do Y" make it flag posts where X never came up (false positives). Word rules as conditions on content that is actually present.
- The risk level is applied at 送審 time from the strategy bound *then* — drafts generated before binding still went to 審核台.

## 審核台

- Newest first; shows 10, then a "顯示其餘 N 篇" button.
- 導流設定 is read-only here. The CTA field is editable but is only written by 核可 (= publishing).
- Work bottom-up if posting order matters (e.g. an Instagram grid).

## 內容計畫 (scheduling)

From the in-app help (`/help?doc=content-plans`): pick a topic, posts per week, number of weeks, start date, publish time (default 19:00) and weekdays; the AI expands topics (1 point), you edit them, then "確認並生成" (1 point per post). Drafts go through review as usual; **核可 schedules them at the planned time**; if that time has passed, 核可 publishes immediately. Timezone = 設定 → 營業時區.

**Untested:** whether plan-generated drafts also default to LINE 導流, and whether their CTA can be set before generation. Schedule one post first and check it in 審核台 before batching.

## 行事曆

Shows published / scheduled / planned posts. Approved posts without a time are supposed to land in a tray for scheduling; in testing, a post approved from the 貼文 flow published immediately instead.

## Automation notes (Claude in Chrome)

- Tutorial overlays (略過導覽 / 開始使用) appear once per page and intercept clicks.
- The draft card's "上傳貼文媒體" is a hidden `<input type=file>`: un-hide it with JS, get its ref from `read_page` (filter: interactive), then `file_upload`. The ref changes for every new draft.
- `find` calls a model and can hit a usage limit during long batches; `read_page` does not.
- A single `javascript_tool` call times out at ~45 s. Wait for generation with `wait` steps, then poll for the draft for at most ~30 s inside JS.
- Sessions expire (you land on `/login`). Injected helper functions are lost on reload or login; re-inject.
- Setting React-controlled inputs: use the native value setter and dispatch `input` + `change`, e.g.
  `Object.getOwnPropertyDescriptor(HTMLInputElement.prototype,'value').set.call(el, v); el.dispatchEvent(new Event('input',{bubbles:true}))`.

## Points (trial, as seen)

Trial: 320 points. AI image 10 points, AI video 30 points; 內容計畫 expand 1 point + 1 point per generated post. Check `/settings/plan` for the current balance.
