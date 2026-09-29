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
- After publishing, the post record may show a stock Chinese CTA for the 自訂 URL track (e.g. "看方案/直接預訂 → <link>") instead of the CTA you set, even though 審核台 showed yours. Nothing is posted while 自動留言 is off; recheck this once auto-comment goes live.
- Alt text isn't part of the composer yet — add it in the Instagram app after publishing.
- On Instagram, comment links show as plain text; the bio link carries the traffic.
- Media: 自行上傳 takes images and **MP4 video** (a 1 MB 1080×1920 Reel worked). Multi-image carousels: not tried yet.
- **Music for Instagram Reels:** posts published through ContentDriven (or any API-based tool) can't use Instagram's music library, and Instagram can't add music to a Reel after it's published. Burn a licensed track into the MP4 before uploading (for business accounts: Meta Sound Collection, licensed for Meta platforms only), or post that Reel from the Instagram app instead.
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
- Media can't be swapped in 審核台. To change the video or image, 退回 the post — a returned post is final (status 已退回, not editable) — and create it again (for a scheduled post: a new one-post 內容計畫 with the same date and time).

## 內容計畫 (scheduling)

Tried end to end on 2026-09-29 with one post:

1. 新規劃 form: topic, posts per week, weeks, start date, publish time (default 19:00), weekdays, length, strategy, and **its own 發布與導流 block** (LINE / 自訂 URL / 自動留言 / CTA). Set 導流 here — it carries into the drafts.
2. 展開題目 costs 1 point and **rewrites your topic** into its own headline. Edit it in the table before confirming (the field saves on a real keystroke + Tab; setting it from script alone doesn't stick).
3. 確認並生成 (1 point per post) → the draft appears on the 貼文 page under 生成結果 after about a minute. Overwrite 發文內容 and upload media there as usual, then 送審.
4. 審核台 shows "這篇來自內容計畫:核可後將排程於 <date time> 發布". 核可 schedules it (a time already in the past publishes right away). Timezone = 設定 → 營業時區.

**Plans publish to Facebook only.** The form has no platform selector, its 各平台形態預覽 shows FB alone, the brand settings have no default-platform option, and choosing IG on the 貼文 page beforehand doesn't carry over (tested 2026-09-29). For Instagram, use the 貼文 page (publishes on approval) or schedule outside ContentDriven.

Also verified on a 2-week × 2-post test (expanded only, not generated):
- Weekday selection works: Wed + Sat from a Monday start gave 10/7, 10/10, 10/14, 10/17, all at the chosen time.
- 整批改發布時間 → 套用到全部 changes every row's time and survives a reload.
- The AI-proposed topics can break brand rules (one was "Debunking Myths: Dreamcatchers Aren't Just for Decoration"). Read every topic before 確認並生成.

**Instagram → Facebook auto-share doesn't cover these posts.** Instagram's "Share to Facebook" only applies to posts made in the Instagram app; posts published through an API (ContentDriven publishes via Zernio) aren't cross-posted (per Buffer/Nuelink docs; not tested here).


## 行事曆

Shows published, scheduled and planned posts, with a tray for approved posts waiting for a time.

## Automating with Claude in Chrome

- `file_upload` can only read files inside folders the Claude session has access to; copy media into the working folder or scratchpad first.

- Each page shows a short tour the first time (略過導覽 / 開始使用); close it before clicking elsewhere.
- After a media upload the draft card re-renders and refs taken before it go stale; click 送審 from script (or take a fresh ref), then confirm the post really left the card.
- The draft card's "上傳貼文媒體" is a hidden `<input type=file>`: un-hide it with JS, get its ref from `read_page` (filter: interactive), then `file_upload`. Each new draft has a new ref.
- Prefer `read_page` over `find` for long batches (`find` uses a model call and counts toward usage).
- A single `javascript_tool` call times out at about 45 s. Wait for generation with `wait` steps, then poll for the draft for up to ~30 s inside JS.
- When the session expires you'll land on `/login`; sign in again and re-inject any helper functions.
- React-controlled inputs: use the native value setter and dispatch `input` + `change`, e.g.
  `Object.getOwnPropertyDescriptor(HTMLInputElement.prototype,'value').set.call(el, v); el.dispatchEvent(new Event('input',{bubbles:true}))`.

## Points (trial, as seen)

Trial: 320 points. AI image 10 points, AI video 30 points. **Text is unlimited**: 內容計畫 says "確認時將扣 N 點", but after several plans (expand + generate) the balance was still 320/320 with nothing in the ledger — text posts fall under 文案吃到飽. Current balance and ledger: `/settings/plan`.
