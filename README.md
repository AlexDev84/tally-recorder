# Tally Recorder

A Plaud-style AI note taker that runs on your iPhone as a home-screen web app.

- **Record** with the iPhone mic. Live timecode and level meter, pause/resume, and **Mark** to flag important moments. The screen stays on while recording.
- **Crash-safe.** Audio is saved in 10-minute parts plus a backup every 30 seconds. If the app is killed, the recording is recovered on next launch.
- **Transcribe** with OpenAI. Speaker labels (Speaker 1, Speaker 2 …, renameable) stay consistent across parts. Supports Hebrew, English and mixed speech. Parts are transcribed *while* you record.
- **Summarize** with Claude using templates: Meeting, Call/1:1, Client discovery, Interview (hiring), Lecture, Doctor visit, Voice memo, or your own. Hebrew recordings get Hebrew summaries.
- **Ask** questions about a recording ("what did they say about budget?", "draft a follow-up email"). Answers include tappable timestamps.
- **Import** audio files (Voice Memos, call recordings, WhatsApp voice notes, any m4a/mp3/wav, long files are split automatically).
- **Share** summaries, a Markdown file with the full transcript, or the audio, through the iOS share sheet.

Everything is stored on the phone (IndexedDB). There is no server of your own; the app talks directly to OpenAI and Anthropic with your keys.

## Files

```
index.html             the whole app
manifest.webmanifest   makes it installable
sw.js                  offline shell
icons/                 home-screen icons
```

## Put it on your iPhone (once, ~5 minutes)

The iPhone only allows microphone access on **https**, so the folder needs free static hosting. Any one of these works:

**Option A — Netlify Drop (no account needed to try, from a computer)**
1. Unzip the folder.
2. Go to https://app.netlify.com/drop and drag the `tally` folder onto the page.
3. You get an `https://….netlify.app` link. Claim the site (free account) so it isn't deleted after an hour.

**Option B — GitHub Pages**
1. Create a new repository and upload the contents of the folder (index.html at the root).
2. Repository › Settings › Pages › Deploy from branch › `main` / root.
3. Your app is at `https://<username>.github.io/<repo>/`.

**Option C — Cloudflare Pages**: Workers & Pages › Create › Pages › Upload assets › drop the folder.

Then on the iPhone:
1. Open the link **in Safari**.
2. Tap **Share › Add to Home Screen**.
3. Open Tally from the home screen, go to **Settings**, paste your keys:
   - OpenAI key — https://platform.openai.com/api-keys (needs a few dollars of credit)
   - Anthropic key — https://console.anthropic.com/settings/keys
4. Tap the red button and allow the microphone.

## Tips

- **Keep the app in front while recording.** iOS pauses the mic for web apps that go to the background or when the phone locks. Tally keeps the screen awake; lower the brightness. If iOS does stop the mic, Tally shows a *Resume recording* button and marks the gap in the transcript.
- **Phone calls:** a web app can't record calls. Use the iPhone's built-in call recording (Phone app, iOS 18.1+, where available), then share the audio file from Notes into Files and **Import** it into Tally.
- **Long imports** (over 20 minutes or 24 MB) are decoded and split on the phone; very long files (3h+) can run out of memory on older iPhones.
- **Back up:** recordings live only in this app on this phone. Use *Export all notes* in Settings, and *Share the audio* for recordings you want to keep. Removing the app from the home screen deletes its data.
- **Cost (approx.):** transcription around $0.36 per hour of audio with GPT-4o Transcribe (half that with the mini model); a Claude summary of an hour-long meeting costs cents.
- **Confidential meetings:** audio goes to OpenAI and transcripts to Anthropic under your API accounts. Check your organization's policy before recording client meetings.

## Updating

Replace `index.html` on the host. To force the offline cache to refresh, change `VERSION` in `sw.js`. Data on the phone is kept.
