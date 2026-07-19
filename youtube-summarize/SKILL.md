---
name: youtube-summarize
description: Download and summarize YouTube video transcripts. Use when the user wants to summarize a YouTube video, get a transcript, understand what a video says, or ask questions about video content.
---

# YouTube Transcript Summarizer

Extract transcripts from YouTube videos, summarize them, and answer follow-up questions.

## Steps

### 1. Confirm prerequisites

```
yt-dlp --version
ffmpeg -version 2>/dev/null && echo "ffmpeg: OK" || echo "ffmpeg: NOT FOUND"
```

**Done when**: yt-dlp prints a version and you know whether ffmpeg is available. If yt-dlp is missing, guide: `brew install yt-dlp` (macOS) or `pip install yt-dlp`.

### 2. Get the video URL

If the user already gave a YouTube URL, use it. Otherwise, ask.

**Done when**: a URL matching `youtube.com/watch` or `youtu.be/` is in hand.

### 3. Check available subtitles

```
yt-dlp --cookies-from-browser chrome --list-subs "<url>" 2>&1
```

Use `chrome` as the default browser. If it fails, try `safari` (macOS), `firefox`, or `brave`. If all fail, retry without `--cookies-from-browser`.

yt-dlp outputs two sections: "Available subtitles" (manual, under a regular heading) and "Available automatic captions" (auto-generated, explicitly labelled). Scan for `en`, `en-orig`, `zh-Hans`, `zh-Hant`, `zh` in manual first, then auto. Report a one-line summary: "Manual: [list] | Auto: [list]". Do not dump the full 100-language listing.

**Done when**: the relevant languages for the priority list are identified and summarized.

### 4. Pick a subtitle language and download

Priority — first match wins:

1. Manual subtitle, original language (check `--list-subs` output for `orig` or the video's primary language line)
2. Manual English (`en`)
3. Manual Chinese (`zh-Hans`, `zh-Hant`, `zh`)
4. Auto-generated English
5. Auto-generated Chinese

If none of these exist, report that no suitable transcript is available and stop.

Download to a temp directory. The command depends on whether ffmpeg is available:

**If ffmpeg is available** — use `--convert-subs srt` which produces clean single-cue SRT:

```
TEMP_DIR=$(mktemp -d) && cd "$TEMP_DIR"
# For manual subs (priority 1-3):
yt-dlp --cookies-from-browser chrome --write-subs --sub-langs "<lang>" --skip-download --convert-subs srt "<url>"
# For auto subs (priority 4-5):
yt-dlp --cookies-from-browser chrome --write-auto-subs --sub-langs "<lang>" --skip-download --convert-subs srt "<url>"
SUBTITLE_FILE=$(ls *.srt 2>/dev/null | head -1)
```

**If ffmpeg is NOT available** — download raw VTT, then immediately clean it with awk (strips timestamps, removes HTML tags, deduplicates overlapping cues):

```
TEMP_DIR=$(mktemp -d) && cd "$TEMP_DIR"
# For manual subs (priority 1-3):
yt-dlp --cookies-from-browser chrome --write-subs --sub-langs "<lang>" --skip-download "<url>"
# For auto subs (priority 4-5):
yt-dlp --cookies-from-browser chrome --write-auto-subs --sub-langs "<lang>" --skip-download "<url>"

# Locate the VTT file and clean it into a clean text file
SUBTITLE_FILE=$(ls *.vtt 2>/dev/null | head -1)
cat "$SUBTITLE_FILE" | awk '
  /^WEBVTT|^Kind:|^Language:|^[[:space:]]*$/ { next }
  /^[0-9]{2}:/ { next }
  { gsub(/<[^>]*>/, ""); gsub(/&amp;/, "\\&"); gsub(/&lt;/, "<"); gsub(/&gt;/, ">"); if ($0 != prev) { print; prev=$0 } }
' > /tmp/yt-transcript-clean.txt
```

Use the same browser as step 3. If `--cookies-from-browser` failed in step 3, omit it here too.

**Done when**: for SRT, `$SUBTITLE_FILE` points to a non-empty `.srt` file. For VTT, `/tmp/yt-transcript-clean.txt` exists and has content.

### 5. Summarize

**If you have an SRT file**: read it directly (SRT is clean, single-cue format). Strip timestamps and sequence numbers — keep only text lines, joining multi-line captions into paragraphs. No dedup needed.

**If you have the cleaned text file** (`/tmp/yt-transcript-clean.txt`): read it directly — it's already clean, deduplicated plain text. If the file is too long, read it with `offset` and `limit` parameters in chunks of ~1500 lines.

**Output in the video's transcript language.** If the transcript is Chinese, write the summary in Chinese; if English, write in English. Do not translate — matching the source language keeps proper nouns and technical terms intact.

Present:

- Video title and channel
- 3-5 key topics with brief descriptions
- Main conclusions or takeaways
- Notable timestamps for key moments (convert SRT timestamps to `mm:ss`)
- **AI's take**: what you found interesting, surprising, or worth deeper investigation. Suggest 2-3 concrete followup actions — topics to research further, related talks to seek out, or experiments to try based on the video's ideas.

**Done when**: the structured summary is presented.

### 6. Q&A

Offer: "Ask me anything about this video."

When the user asks a question, answer from the transcript. Quote relevant lines when helpful. If the answer isn't in the transcript, say so.

**Done when**: the user signals they're finished (explicitly or by starting an unrelated topic).

## Fallbacks

- **No subtitles at all**: report and stop. If the video might have a transcript but yt-dlp can't see it (e.g. region-locked), suggest the user try a VPN or a different browser for cookies.
- **Subtitle file empty after download**: retry with the next language in the priority list.
- **Transcript too long to fit in one read**: save cleaned text to `/tmp/yt-transcript-clean.txt`, then read it in chunks — summarize each half separately, then merge.
