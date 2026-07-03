# Grok Imagine Video 1.5 API with APIDot

Build with the Grok Imagine Video 1.5 API using APIDot: cURL, Node.js, request shapes, polling, webhooks, and production notes.

[Get API Key](https://apidot.ai/dashboard/api-key) | [API Docs](https://apidot.ai/docs/grok-imagine-video-1-5) | [Model Page](https://apidot.ai/models/grok-imagine-video-1-5) | [Main Examples](https://github.com/APIDotAI/apidot-examples)

## What this example shows

- How to submit a Grok Imagine Video 1.5 image-to-video job through APIDot.
- How to send the required prompt, one source image URL, resolution, and duration.
- How to store `data.task_id`, poll status, and use webhook callbacks.
- How to avoid mixing this image-to-video shape with text-to-video payloads.

## When to use it

Use this repo when you already have one source image and want to animate it into a short prompt-guided clip. It is a good starting point for product motion tests, social previews, storyboard beats, and other workflows where the input image should anchor the generated video.

Do not use this request shape for prompt-only text-to-video. Grok Imagine Video 1.5 on this APIDot page requires exactly one `input.image_urls` value.

## Requirements

- An APIDot account.
- An APIDot API key stored server-side.
- Node.js 18 or newer for the Node.js example.
- `curl` for the cURL example.
- One public image URL reachable by APIDot.

## Environment variables

Use placeholders only. Do not commit real credentials.

```env
APIDOT_API_KEY=YOUR_API_KEY_HERE
APIDOT_BASE_URL=https://api.apidot.ai
# Optional: set only when you have a real public webhook receiver.
# APIDOT_CALLBACK_URL=https://example.com/api/apidot/webhook
```

## How to run

```bash
cp .env.example .env
# Edit .env and set APIDOT_API_KEY
cd node
npm start
```

Use [curl/generate.md](curl/generate.md) when you want a copy-paste cURL request instead of the Node.js polling example.

## Minimal API request

Submit to APIDot's unified async generation endpoint:

```json
{
  "model": "grok-imagine-video-1.5",
  "callback_url": "https://example.com/api/apidot/webhook",
  "input": {
    "prompt": "Animate this product photo with a smooth push-in, soft studio reflections, and subtle background motion.",
    "image_urls": [
      "https://example.com/source-image.webp"
    ],
    "resolution": "720p",
    "duration": 6
  }
}
```

## Request variants

### Draft preview

```json
{
  "model": "grok-imagine-video-1.5",
  "input": {
    "prompt": "Turn this product image into a short motion preview with gentle camera movement.",
    "image_urls": [
      "https://example.com/source-image.webp"
    ],
    "resolution": "480p",
    "duration": 4
  }
}
```

### Sharper review clip

```json
{
  "model": "grok-imagine-video-1.5",
  "input": {
    "prompt": "Animate the object with a controlled orbit shot, realistic reflections, and a calm premium mood.",
    "image_urls": [
      "https://example.com/product-reference.png"
    ],
    "resolution": "720p",
    "duration": 8
  }
}
```

## Request parameters

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| model | string | yes | Use `grok-imagine-video-1.5`. |
| callback_url | string | no | Optional webhook callback URL for terminal task updates. |
| input.prompt | string | yes | Motion prompt describing subject action, camera movement, pacing, and mood. |
| input.image_urls | string[] | yes | Exactly one source image URL. |
| input.resolution | string | no | Supported values are `480p` and `720p`. |
| input.duration | integer | no | Supported integer values are `1` through `15`. |

## Expected response

Submit response:

```json
{
  "code": 200,
  "data": {
    "task_id": "task-unified-example",
    "status": "not_started",
    "created_time": "2026-04-19T21:19:42"
  }
}
```

Shortened finished response:

```json
{
  "code": 200,
  "data": {
    "task_id": "task-unified-example",
    "status": "finished",
    "output": {
      "files": [
        {
          "file_url": "https://example.com/generated-video.mp4",
          "file_type": "video"
        }
      ]
    }
  }
}
```

## Production notes

- Keep APIDot API keys in server-side environment variables.
- Persist `task_id`, source image URL, prompt, selected resolution, duration, user ID, and status together.
- Validate that the source image URL is public and long-lived enough for processing.
- Use polling for local tests and webhook callbacks for production queues.
- Avoid logging API keys, private prompts, private media URLs, or production callback URLs.
- Retry transient network failures with backoff, but do not retry unchanged invalid payloads.

## Common mistakes

- Sending no image URL or more than one image URL.
- Treating this endpoint as text-to-video.
- Using private, signed, or short-lived image URLs.
- Sending unsupported `resolution` values.
- Losing `data.task_id` before polling or webhook reconciliation.
- Committing a real `.env` file or API key.

## Related links

- Website: https://apidot.ai
- Docs: https://apidot.ai/docs
- GitHub: https://github.com/APIDotAI
- Grok Imagine Video 1.5 model page: https://apidot.ai/models/grok-imagine-video-1-5
- Grok Imagine Video 1.5 API docs: https://apidot.ai/docs/grok-imagine-video-1-5
- APIDot quickstart: https://apidot.ai/docs/quickstart
- APIDot webhooks: https://apidot.ai/docs/webhooks
- Main APIDot examples repo: https://github.com/APIDotAI/apidot-examples

## Promotion

- Topic: Grok Imagine Video 1.5 image-to-video request shape and async APIDot workflow.
- Audience: Backend developers adding prompt-guided image-to-video generation to product workflows.
- Tweet angle: Grok Imagine Video 1.5 is not prompt-only; start with exactly one image URL, then choose resolution and duration before polling.
- Landing page: https://apidot.ai/models/grok-imagine-video-1-5
- Source status: verified

## Quality review

- Quality score: 38/40
- Promotion candidate: yes
- Blocking issues: None for request-shape and async workflow promotion. Verify the model page and docs again before making cost, ranking, or availability claims.
