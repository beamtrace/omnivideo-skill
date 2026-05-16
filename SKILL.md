---
name: omnivideo
description: Generate video and image content via the Omni Video API (https://omnivideo.net/) — Gemini Omni Video series of models (seedance-2 for video, gpt-image-2 / nano-banana-2 for images). Use when the user asks to create a video or image from a prompt with Omni Video, generate a video clip from text/image, render an AI image with the Gemini Omni Video models, or mentions seedance-2 / gpt-image-2 / nano-banana-2. Triggers on phrases like "Omni Video", "omnivideo", "Gemini Omni Video", "generate a video with omnivideo", "draw an image with omni video", "render with seedance-2".
---

# Omni Video

Call the [Omni Video](https://omnivideo.net/) REST API to generate video or image content with the **Gemini Omni Video** family of models — `seedance-2` (text/image → video), `gpt-image-2` and `nano-banana-2` (text/image → image).

> **Get an API key first.** Tell the user to sign in at <https://omnivideo.net/> and copy a `sk-…` token from the account page. Read it from `OMNIVIDEO_API_KEY` so the key never appears in chat or code.

## When to use

Trigger this skill whenever the user wants to:

- Generate a short AI video from a text prompt (and optionally reference images).
- Generate an image from a text prompt (and optionally reference images).
- Inspect or poll an Omni Video task they already submitted.
- Compare the three Gemini Omni Video models or pick the right one.

## Decision tree

1. **What output does the user want?**
   - Video → use `model_id: "seedance-2"`.
   - Image → default to `model_id: "gpt-image-2"`; use `nano-banana-2` if the user asks for a cheaper/faster image alternative.
2. **Do they have reference images?**
   - Yes → pass `image_urls: [url, ...]`.
   - No → omit the field (text-to-X).
3. **Aspect ratio?**
   - Default to `16:9` for video and `1:1` for images unless the user specifies otherwise (e.g. `9:16` portrait, `4:5`).
4. **Run it.** Use the convenience `run` helper if available in the chosen SDK; otherwise create + poll until `task_status` is 3 (success) or 4 (failed).

## Workflow

1. Confirm the user has set `OMNIVIDEO_API_KEY`. If not, link them to <https://omnivideo.net/> to issue one.
2. Pick the SDK that matches their stack — see [`references/api.md`](references/api.md) for raw REST or [`references/sdk-snippets.md`](references/sdk-snippets.md) for ready-to-paste code in Python, Node/TS, Ruby, Go and PHP.
3. Build the prompt with the user — Omni Video prompts behave like other diffusion models: subject, style, lighting, camera, modifiers.
4. Submit, poll, then surface the resulting `video_url` or `image_url`.

## Models at a glance

| `model_id`      | Modality           | Output      | Notes                                 |
| --------------- | ------------------ | ----------- | ------------------------------------- |
| `seedance-2`    | text/image → video | `video_url` | The Gemini Omni Video video model.    |
| `gpt-image-2`   | text/image → image | `image_url` | High-fidelity image (Gemini Omni).    |
| `nano-banana-2` | text/image → image | `image_url` | Lighter/faster image variant.         |

## API surface (summary)

`POST /api/v1/tasks/create` with `{ model_id, prompt, image_urls?, aspect_ratio? }` → returns `task_id`.
`GET /api/v1/tasks/{task_id}` → returns the current state. `task_status`: 1=queued, 2=running, 3=success, 4=failed.

Auth header on every request: `Authorization: Bearer sk-...`.

## Reference

- Full endpoint reference: [`references/api.md`](references/api.md)
- Per-language snippets: [`references/sdk-snippets.md`](references/sdk-snippets.md)
- Omni Video website (sign in, get key): <https://omnivideo.net/>
- Live API docs: <https://omnivideo.net/api-docs>
