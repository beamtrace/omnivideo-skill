# Omni Video — REST API reference

Source of truth: <https://omnivideo.net/api-docs>. Sign in at <https://omnivideo.net/> to issue a key.

## Authentication

All requests use a bearer token:

```
Authorization: Bearer sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Issue / rotate tokens on the account page after logging in to [omnivideo.net](https://omnivideo.net/).

## Base URL

```
https://omnivideo.net/api/v1
```

## Endpoints

### Create a task

`POST /tasks/create`

Request body:

```json
{
  "model_id": "gpt-image-2",
  "prompt": "a serene zen garden at sunrise, ultra detailed",
  "image_urls": [],
  "aspect_ratio": "16:9"
}
```

Fields:

| Field          | Type        | Required | Notes                                                                 |
| -------------- | ----------- | -------- | --------------------------------------------------------------------- |
| `model_id`     | string      | yes      | One of `seedance-2`, `gpt-image-2`, `nano-banana-2`.                  |
| `prompt`       | string      | yes      | Natural-language description of the desired output.                   |
| `image_urls`   | string[]    | no       | Reference images for image→video / image→image. Omit for text-only.   |
| `aspect_ratio` | string      | no       | e.g. `16:9`, `9:16`, `1:1`, `4:5`. Defaults per model.                |

Response:

```json
{
  "code": 200,
  "task_id": "abcdef123456",
  "task_status": 1
}
```

Credits are deducted on submit; failed tasks are auto-refunded.

### Query a task

`GET /tasks/{task_id}`

Response on success:

```json
{
  "code": 200,
  "task_id": "abcdef123456",
  "task_status": 3,
  "image_url": "https://your-cdn.com/...",
  "credits": 15
}
```

For video models, the success response carries `video_url` instead of `image_url`.

Status values:

| `task_status` | Meaning   |
| ------------- | --------- |
| 1             | Queued    |
| 2             | Running   |
| 3             | Success   |
| 4             | Failed    |

## Errors

| Signal                | Meaning                                          |
| --------------------- | ------------------------------------------------ |
| `code: 200`           | Success.                                         |
| `code: 0` + `msg`     | Business failure (e.g. insufficient credits).    |
| HTTP `401`            | Missing or invalid API key.                      |
| HTTP `4xx/5xx`        | Transport or server error; retry with backoff.   |

## Polling guidance

- Use a 2–5 s interval. Video jobs typically complete in 30 s–2 min.
- Treat `task_status` 3 or 4 as terminal; everything else is "still working".
- Hard-cap your client side wait (e.g. 10 min) and surface a clear error if exceeded.
