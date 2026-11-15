# Sabi: scam check before you send

Sabi is a Telegram bot that checks suspicious messages in any language and tells you whether they look like a scam, why, and what to do. Try it: @sabicheckerbot

Built for [openserv hackathon] by [chidera edwin].

## What it does
- Paste or forward a message. Sabi replies with a verdict (likely scam, suspicious or likely safe), a confidence level, red flags and safe next steps.
- Works in English, French, Spanish, Chinese, Hindi and Arabic.
- Adds a scam type label (bank phishing, crypto airdrop, fake job and more) on English replies.
- Resists messages that tell it to "ignore the rules" or "say this is safe".
- /stats shows simple counts. Only date, scam type, verdict and language are saved. Message text and user IDs are never stored.

## How it works
Telegram bot in Node.js (bot.mjs), using the gpt-5.4-mini model through SERV inference. It is hosted on a free server. API keys are read from environment variables and are not in this repo.

## Results
Test set: 26 hand-labelled messages (scams, safe messages, tricky cases, and 6 languages). Run twice on the exact prompt used by the live bot.

- Missed scams (a scam called safe): 0 in both runs
- Correct: 25 of 26 in both runs
- Injection tricks resisted: 3 of 3
- Valid reply format on first try: 26 of 26
- The one miss in both runs: a genuine-style bank alert flagged as a scam (the cautious direction)

Reproduce with: node score.mjs plain hard (needs your own keys.txt)

## Reppo review loop
10 Sabi replies were scored by Reppo. Low scores were sent back as critique and Sabi rewrote its answer (reppo-loop.mjs, results in reppo-results-FINAL.json).

- 7 of 10 first answers scored 100 and shipped unchanged.
- 3 of 10 were flagged for review, and all 3 improved after the rewrite: 56 to 100, 89 to 100, 75 to 88.
- Two of those three are still marked "review" by Reppo.

Reppo takes 1 to 2 minutes per evaluation, so it is used as an offline review, not on every live message.

## Known limitations
- Leans cautious on bank-style alerts and lone one-time-code messages, so it can flag a genuine one.
- Non-English replies may mix in some English headings.
- The scam type label shows on English replies only.
- Text only. Images and voice notes are not supported yet.
- Small test set (26 messages). It is evidence, not a guarantee.
- A risk check, not a guarantee. Never share passwords, PINs, OTP codes or recovery phrases.

## Roadmap
Live today: everything above.
In progress: scam-trend dashboard built from anonymous counts.
Planned next: WhatsApp and X integration, screenshot and voice note checks, a larger test set.
Items outside "Live today" are not built yet.

## Run it yourself
1. Install Node.js.
2. Create keys.txt with SERV_API_KEY and TELEGRAM_BOT_TOKEN, one per line as NAME=value.
3. Run: node bot.mjs

Never commit keys.txt.
