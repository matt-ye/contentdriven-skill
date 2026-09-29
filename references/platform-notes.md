# ContentDriven — operating tips

Collected on app.contentdriven.ai on 2026-09-28/29 while staging a 15-post Instagram launch (trial plan). The platform keeps improving; if something here no longer matches what you see, go with the screen and update this file.

## How a post moves through the platform

`命題 → 生成 → 草稿卡 → 送審 → 審核台 → 核可 → published (or scheduled)`

| Step | What gets saved | Tip |
|---|---|---|
| 生成 | The draft, **plus the 導流 settings and 留言 CTA exactly as they are in the composer at that moment** | Set 導流 and the CTA first, then press 生成. To change them later, generate the post again (text generation is quick). |
| Editing 發文內容 on the draft card | Saved when you press 送審 | Finish your edits and submit in one go; a page reload starts the draft card fresh. Media uploads are kept right away. |
| 修改指示 → 送出修改 | An AI revision of the post | Use it for "make it warmer / shorter"; for word-for-word text, paste it into 發文內容 instead. |
| 送審 | The caption as it is in the box, media, 導流 and CTA | Then the bound strategy's risk level decides: low-risk posts are approved and published automatically; everything else goes to 審核台. |
| 核可 | Publishes | Posts created on the 貼文 page publish as soon as they're approved. For a set time, create them through 內容計畫 — approval then schedules them. |

## Composer (貼文 page)

- **The composer opens with LINE-first defaults** (LINE 諮詢 on, 自動留言 on, a Chinese LINE CTA). For other destinations, adjust 導流 before each 生成.
- 導流 options: LINE 諮詢連結 (for brands with a LINE OA), 自訂 URL (any link, wrapped in a click-counting short link), or both off (impressions only).
- **The generator writes its own take on the caption**, even when the topic asks for exact wording — it tends to add emoji, a closing CTA sentence, or example details. For approved copy, paste it into 發文內容 before 送審, and double-check any facts or numbers against your source.
- 留言內容 (CTA) is a single line.
- 發布後自動留言 is marked 即將生效 (coming soon). Until it's live, post first comments yourself.
- Alt text isn't part of the composer yet — add it in the Instagram app after publishing.
- On Instagram, comment links show as plain text; the bio link carries the traffic.
- Media: 自行上傳 takes PNG/JPG (1.2 MB files worked). Multi-image carousels: not tried yet.
- Generation takes about 50–75 seconds per post.

## Strategy (策略庫)

- A strategy is a Markdown file with frontmatter `vertical` and `review_risk`, and sections 角色／禁詞／內容主題／結構／範例／合規規則. See `strategy-template.md`.
- Put **one 禁詞 per line**; a comma-separated line is read as a single term.
- `review_risk: high` sends every submission to 審核台 — the right choice when a human should see every post before it goes out.
- The compliance check reads the strategy's 合規規則 literally. Rules phrased as conditions on what a post actually contains ("if a post mentions X, it must also Y") give cleaner flags than "whenever X comes up…".
- The risk level is applied when you press 送審, using the strategy bound at that moment.

## 審核台

- Newest first; the first 10 are shown, then "顯示其餘 N 篇".
- 導流設定 is shown for reference here; the CTA field is saved together with 核可.
- If posting order matters (e.g. an Instagram grid), approve from the bottom up.

## 內容計畫 (scheduling)

From the in-app help (`/help?doc=content-plans`): choose a topic, posts per week, number of weeks, start date, publish time (default 19:00) and weekdays; the AI proposes topics (1 point), you edit them, then "確認並生成" (1 point per post). Drafts go through review as usual, and **核可 schedules each at its planned time** (a time already in the past publishes right away). Timezone = 設定 → 營業時區.

Not tried yet: how 導流 and the CTA are set for plan-generated drafts. Schedule one post first and check it in 審核台 before doing a batch.

## 行事曆

Shows published, scheduled and planned posts, with a tray for approved posts waiting for a time.

## Automating with Claude in Chrome

- Each page shows a short tour the first time (略過導覽 / 開始使用); close it before clicking elsewhere.
- The draft card's "上傳貼文媒體" is a hidden `<input type=file>`: un-hide it with JS, get its ref from `read_page` (filter: interactive), then `file_upload`. Each new draft has a new ref.
- Prefer `read_page` over `find` for long batches (`find` uses a model call and counts toward usage).
- A single `javascript_tool` call times out at about 45 s. Wait for generation with `wait` steps, then poll for the draft for up to ~30 s inside JS.
- When the session expires you'll land on `/login`; sign in again and re-inject any helper functions.
- React-controlled inputs: use the native value setter and dispatch `input` + `change`, e.g.
  `Object.getOwnPropertyDescriptor(HTMLInputElement.prototype,'value').set.call(el, v); el.dispatchEvent(new Event('input',{bubbles:true}))`.

## Points (trial, as seen)

Trial: 320 points. AI image 10 points, AI video 30 points; 內容計畫: 1 point to propose topics + 1 point per generated post. Current balance: `/settings/plan`.
