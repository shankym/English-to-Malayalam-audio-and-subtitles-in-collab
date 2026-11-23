# 🎬 English → Malayalam Video Dubbing (free, in your browser)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/shankym/English-to-Malayalam-audio-and-subtitles-in-collab/blob/main/English_to_Malayalam_Dubbing.ipynb)

Turn an English video in your Google Drive into a **Malayalam-dubbed video with Malayalam subtitles**. It is completely free, needs nothing installed on your computer, and keeps the original picture quality.

It runs in **Google Colab** on Google's free GPU. You paste a Drive link, press a few buttons, and the finished video appears back in your Drive.

---

## ✨ What you get

| Output | Details |
|---|---|
| 🗣️ **Malayalam voice** | Natural male or female neural voice, timed to each sentence of the original |
| 🎵 **Background kept** | Music and sound effects are separated out and kept under the new voice |
| 🔁 **English kept** | The original English audio stays as a second track you can switch to |
| 💬 **Malayalam subtitles** | Soft subtitles you can turn on and off (not burned into the picture) |
| 🎞️ **Same picture** | The video stream is copied as it is, with no re-encoding and no quality loss |
| ✍️ **Natural language** | Optional Gemini step rewrites the translation into everyday spoken Malayalam |

Everything is saved to **My Drive → `Malayalam_Dubbing` → `<video name>_malayalam`**:

```
<name>_malayalam.mkv               ← final video (video + Malayalam audio + English audio + subtitles)
<name>_malayalam.srt               ← Malayalam subtitles (the ones used for the voice)
<name>_malayalam_indictrans.srt    ← raw machine translation (before Gemini)
<name>_english.srt                 ← English transcript
<name>_malayalam_audio.mp3         ← Malayalam audio track on its own
```

---

## 🧰 How it works

| Step | Tool (all free and open) | What it does |
|---|---|---|
| 1 | — | Installs everything, then restarts the session automatically |
| 2 | Google Drive API | Copies your video from Drive (byte-for-byte, no quality loss) |
| 3 | [Whisper large-v3](https://github.com/SYSTRAN/faster-whisper) | English speech → timed English subtitles |
| 4 | [IndicTrans2](https://github.com/AI4Bharat/IndicTrans2) by AI4Bharat | English → Malayalam translation, built for Indian languages |
| 5 | [Gemini](https://aistudio.google.com) free tier (optional) | Rewrites the translation into natural, spoken Malayalam |
| 6 | — | You check the subtitles and fix any line |
| 7 | [Demucs](https://github.com/facebookresearch/demucs) | Separates music and sound effects from the English voices |
| 8 | [edge-tts](https://github.com/rany2/edge-tts) | Malayalam voice for each line, fitted to its time slot, mixed over the music |
| 9 | [FFmpeg](https://ffmpeg.org) | Packs it all into one `.mkv` without touching the picture |

---

## ✋ One-time setup (about 5 minutes)

You need a **Google account** and two free keys.

### 1. Hugging Face token (required)
The translation model is free but asks you to accept its terms once.
1. Sign up at **[huggingface.co/join](https://huggingface.co/join)** (you can use Google).
2. Open **[ai4bharat/indictrans2-en-indic-1B](https://huggingface.co/ai4bharat/indictrans2-en-indic-1B)** and click **Agree and access repository**.
3. Go to **[huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)** → **Create new token**.
4. Click the **Read** tab at the top (not "Fine-grained"), name it `colab`, click **Create token**.
5. Copy the token. It starts with `hf_` and is shown only once.

> Only see the "Fine-grained" form? That works too: tick **"Read access to contents of all public gated repos you can access"** and create it. If the Create button is grey, confirm your email address first.

### 2. Gemini API key (optional, recommended)
Makes the Malayalam sound like everyday speech instead of textbook language.
1. Open **[aistudio.google.com](https://aistudio.google.com)** and sign in with Google.
2. Click **Get API key → Create API key**, and copy it.

No credit card is needed. The free tier easily covers a 10-minute video.

### 3. Save both keys in Colab Secrets
This keeps your keys out of the notebook, so you can never share them by accident.
1. Open the notebook in Colab (use the **Open in Colab** button above).
2. Click the **🔑 key icon** in the left sidebar.
3. **Add new secret** → Name: `HF_TOKEN` → Value: your Hugging Face token → switch on **Notebook access**.
4. **Add new secret** → Name: `GEMINI_API_KEY` → Value: your Gemini key → switch on **Notebook access**.

Colab remembers these secrets for your account, so you only do this once.

---

## ▶️ Dubbing a video

1. **Open the notebook** with the **Open in Colab** button.
2. **Turn on the GPU:** menu **Runtime → Change runtime type → T4 GPU → Save**.
3. **Run Step 1** (press ▶). It takes 3–5 minutes and then **restarts the session by itself**. A *"session restarted"* or *"session crashed"* message is expected.
4. **Get your video link:** in Google Drive, right-click the video → **Share → Copy link**.
5. **Fill in Step 2:** paste the link into `VIDEO_LINK`. Pick the voice (male or female), music volume and options.
6. **Click inside Step 2 and press `Ctrl + F10`** (or menu **Runtime → Run cell and below**). This runs Steps 2–9 one after another.
7. **Allow the Google pop-ups.** There are two: one to read your video, one to save results to Drive.
8. **Wait.** A 6-minute video takes about **15–20 minutes** (most of it is the first-time model downloads).
9. **Open your Drive** → `Malayalam_Dubbing` → your folder, and download the `.mkv`.

### Watching the result
Use **[VLC](https://www.videolan.org/)** (free, on PC, Mac, Android and iPhone):
- **Audio → Audio Track**: switch between Malayalam and English
- **Subtitle → Sub Track**: turn Malayalam subtitles on or off

Google Drive's built-in player only plays the first audio track and does not show these subtitles, so check the result in VLC.

---

## ✏️ Fixing the Malayalam

After a run, read the list printed by **Step 6**. Each Malayalam line is shown with its English line underneath.

**To fix a line**, add it to `FIXES` in Step 6:
```python
FIXES = {
    5: 'നിങ്ങളുടെ തിരുത്തിയ വാചകം',
    12: 'മറ്റൊരു തിരുത്ത്',
}
```
Run Step 6, then **Step 8 and Step 9** again. Only the lines you changed get a new voice, so this is quick.

You can also edit the subtitle file directly: **📁 Files panel → drive → MyDrive → Malayalam_Dubbing → your folder → double-click `<name>_malayalam.srt`**, edit the text (leave the numbers and times alone), press **Ctrl+S**, then run Step 6, 8 and 9.

---

## 🔁 Common tasks

| I want to… | Do this |
|---|---|
| Dub another video | Paste the new link in Step 2 → click in Step 2 → `Ctrl + F10`. No need to run Step 1 again in the same session. |
| Change voice or music volume | Change it in Step 2 → run Step 2 → run Step 8 → Step 9 |
| Make lines sound less rushed | Shorten those lines in Step 6 → run Step 8 → Step 9 |
| Redo a step from scratch | Tick its **REDO** box and run it. Step 5's REDO removes your Step 6 fixes. |
| Continue another day | Run Step 1 (Colab forgets installs after it disconnects), then Step 2 → `Ctrl + F10`. Finished work saved in Drive is reused. |

**Every step remembers its work.** If anything stops halfway, run that step again; steps that already finished are skipped.

---

## 🛠️ Troubleshooting

| Message | What it means / what to do |
|---|---|
| Red *"pip's dependency resolver… conflicts"* | Normal while installing. Ignore it. |
| *"Your session crashed"* / *"restarted"* after Step 1 | Normal. Step 1 restarts the session on purpose so the new tools load cleanly. Go to Step 2. |
| *"No GPU"* | Runtime → Change runtime type → T4 GPU. At busy times the free GPU may be unavailable; try again later. |
| *"Run Step 1 first"* / *"Run Step 2 first"* | Run that step, then continue. |
| *"unauthenticated requests to the HF Hub"* | Only a warning. Ignore it. |
| *"Hugging Face token missing"* | Add the `HF_TOKEN` secret (🔑) with Notebook access switched on, run Step 2 again, then Step 4. |
| *"No access to the translation model"* | Open the IndicTrans2 page with the same Hugging Face account and click **Agree and access repository**. |
| *"Could not open that file"* | The video must be in, or shared with, the Google account you signed in with. Check the link. |
| Gemini *429* / *RESOURCE_EXHAUSTED* | Free-tier limit. The step waits and retries; if needed it keeps the plain translation for those lines. |
| *"The voice service did not respond"* | Temporary internet issue. Run Step 8 again; finished lines are reused. |
| Colab disconnected during a long run | Run Step 1, then Step 2 → `Ctrl + F10`. Completed steps are skipped. |

---

## ⚠️ Limitations

- **One voice for everyone.** All speakers are voiced by the same Malayalam voice. Choose male or female to suit most of the speakers.
- **No lip-sync.** The voice is timed to each sentence, not to lip movements.
- **Malayalam often runs longer than English.** Long lines are spoken up to ~40% faster to fit. If a line sounds rushed, shorten it in Step 6.
- **Always review the translation.** Machine translation can get names, terms or tone wrong; a quick read by a Malayalam speaker makes a big difference.
- **edge-tts** uses Microsoft Edge's online "Read aloud" voices through an unofficial library. It is free but may change or become unavailable without notice.
- Tested with videos up to about 10 minutes on Colab's free T4 GPU. Much longer videos may hit Colab's memory or time limits.

---

## 🔒 Privacy and keys

- Your video is processed inside **your own** Colab session and saved to **your own** Drive.
- Text of the subtitles is sent to Google's Gemini API (if enabled) and to Microsoft's voice service (for the speech).
- Keep your keys in **Colab Secrets**. If you ever type keys into the Step 2 form, **don't save that copy to GitHub or share it**.
- If a key has been exposed, delete it and create a new one ([Hugging Face tokens](https://huggingface.co/settings/tokens), [Gemini keys](https://aistudio.google.com/apikey)).

---

## 🙏 Credits

This notebook glues together excellent free projects:
[OpenAI Whisper](https://github.com/openai/whisper) / [faster-whisper](https://github.com/SYSTRAN/faster-whisper) ·
[AI4Bharat IndicTrans2](https://github.com/AI4Bharat/IndicTrans2) and [IndicTransToolkit](https://github.com/VarunGumma/IndicTransToolkit) ·
[Demucs](https://github.com/facebookresearch/demucs) ·
[edge-tts](https://github.com/rany2/edge-tts) ·
[Google Gemini](https://ai.google.dev) ·
[FFmpeg](https://ffmpeg.org).
Each keeps its own license and terms of use.

## 📄 License

The notebook code is released under the [MIT License](LICENSE).
