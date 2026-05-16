# Omni Video — Claude Code Skill

A Claude Code skill that lets Claude generate video and image content via the [Omni Video](https://omnivideo.net/) REST API, covering the **Gemini Omni Video** family of models — `seedance-2` (text/image → video), `gpt-image-2` and `nano-banana-2` (text/image → image).

> Sign in at <https://omnivideo.net/> and create an `sk-…` API key on the account page. Export it as `OMNIVIDEO_API_KEY` and Claude will pick it up automatically.

## What this skill does

When triggered, Claude will:

1. Pick the right model for you (`seedance-2` for video, `gpt-image-2` for high-fidelity images, `nano-banana-2` for cheaper/faster image generation).
2. Help you sharpen the prompt (subject + style + lighting + camera + modifiers).
3. Submit the job through any of the official Omni Video SDKs (Python / Node / Ruby / Go / PHP) and poll until done.
4. Return the final `video_url` or `image_url`.

The full workflow and model matrix lives in [SKILL.md](./SKILL.md) and [references/](./references/).

## Install into Claude Code

Clone the repo into your Claude Code skills directory:

```bash
# User-level (applies to every project)
git clone https://github.com/beamtrace/omnivideo-skill.git ~/.claude/skills/omnivideo

# Or project-level (only the current project)
git clone https://github.com/beamtrace/omnivideo-skill.git .claude/skills/omnivideo
```

Restart Claude Code and ask something like *"generate a short video of … with Omni Video"* — the skill should fire automatically.

## How to trigger it

The skill's `description` field already matches the most common phrasings. Any of these will trigger it:

- "generate a video with omnivideo"
- "draw an image using omni video"
- "how do I call Gemini Omni Video"
- "use seedance-2 to make a clip"
- mentioning any of `seedance-2`, `gpt-image-2`, `nano-banana-2`

## Companion multi-language SDKs

| Language  | Package                       | Registry                                                                  |
| --------- | ----------------------------- | ------------------------------------------------------------------------- |
| Python    | `omnivideo-sdk`               | [PyPI](https://pypi.org/project/omnivideo-sdk/)                           |
| Node / TS | `omnivideo-sdk`               | [npm](https://www.npmjs.com/package/omnivideo-sdk)                        |
| Ruby      | `omnivideo-sdk`               | RubyGems                                                                  |
| Go        | `omnivideo-sdk-go`            | [github.com/beamtrace/omnivideo-sdk-go](https://github.com/beamtrace/omnivideo-sdk-go) |
| PHP       | `omnivideo/omnivideo-sdk`     | [Packagist](https://packagist.org/packages/omnivideo/omnivideo-sdk)       |

Each SDK is a ~150-line thin wrapper that exposes the same three methods: `create_task` / `get_task` / `run`.

## Links

- Website (sign up + get key): <https://omnivideo.net/>
- Live API reference: <https://omnivideo.net/api-docs>

## License

MIT
