# ExplifyAI

**Upload your class notes and get a personal video lesson made for what *you* don't understand yet, with an AI teacher you can stop and ask questions at any moment.**

[![Watch the ExplifyAI demo](media/dns-demo.jpg)](media/explifyai-demo.mp4)

▶️ **[Watch the demo](media/explifyai-demo.mp4)** · [Sample lesson](media/dns-demo.mp4)

> The product's source code is private. This repo shows what it does and how it works.

## The problem

Indian college students study from dense notes and slides, and then cram. YouTube explains *a* topic, but not *their* syllabus at *their* level. Coaching is expensive. Chatbots give walls of text, not understanding.

## What ExplifyAI does

1. **Upload notes**: PDFs, slides or photos of handwritten pages. OCR reads them and builds a topic map.
2. **Find the gap**: a short adaptive diagnostic works out how deep the student's understanding already goes, topic by topic.
3. **Personal video**: a narrated, animated lesson is generated for that student. It moves up Bloom's taxonomy (remember → understand → apply → analyse) starting from where they are.
4. **Ask the teacher**: pause anywhere and ask. The AI teacher knows the exact moment of the video you're on.
5. **Lock it in**: a quiz and flashcards made from the same lesson.

Lessons can be made in **16 Indian languages**, including Tamil, Hindi and Tanglish.

## How a video is generated

```
notes ──► OCR + topic map ──► depth diagnostic ──► storyboard (1 LLM call: beats, Bloom level, visuals)
                                                         │
                         ┌───────────────────────────────┼───────────────────────────────┐
                         ▼                               ▼                               ▼
                    scene 1                          scene 2               …         scene N      (in parallel)
          voice + word-level cue timing → LLM writes the scene → automated layout checks
          → rule-based fixes (free) → LLM patch only if still needed → render
                         └───────────────────────────────┬───────────────────────────────┘
                                                         ▼
                                                 merge (ffmpeg) ──► lesson
```

- **Cheap by design**: small, fast models do most of the work, and a larger model is used only for planning and as a last resort. The cost of every lesson is measured and logged.
- **Resumable**: an interrupted render never pays for the same LLM call twice.
- **Self-checking**: an in-page runtime fits text to the space, places annotations and prevents overlaps before a frame is rendered. A quality tool scores every video without anyone watching it.
- **Scales to the machine**: the number of parallel workers is picked from the available CPU and RAM.

## Stack

| Layer | Tech |
|---|---|
| Frontend | Next.js · TypeScript · Tailwind · shadcn/ui · Motion |
| Backend | Python · FastAPI |
| AI | Gemini (planning, scene code, OCR, quiz, chat) |
| Voice | Kokoro TTS · Whisper alignment for word-level timing |
| Video | HTML-based scene engine → headless render → ffmpeg |
| Infra | AWS (Ubuntu) · systemd · auto-deploy on push |

## Founder

**Sakthivel R**, CS undergraduate at Rajalakshmi Engineering College, Chennai. [LinkedIn](https://www.linkedin.com/in/sakthivel-r-957a87318/)
