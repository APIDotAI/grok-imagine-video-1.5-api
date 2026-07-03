# Grok Imagine Video 1.5 cURL Quickstart

## What this example shows

This example submits one image-to-video task and then polls the shared APIDot task status endpoint.

## How to run

```bash
export APIDOT_API_KEY="YOUR_API_KEY_HERE"

curl --fail-with-body --request POST \
  --url https://api.apidot.ai/api/generate/submit \
  --header "Authorization: Bearer $APIDOT_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{
    "model": "grok-imagine-video-1.5",
    "input": {
      "prompt": "Animate this product photo with a smooth push-in, soft studio reflections, and subtle background motion.",
      "image_urls": [
        "https://example.com/source-image.webp"
      ],
      "resolution": "720p",
      "duration": 6
    }
  }'
```

Store the returned `data.task_id`, then poll status:

```bash
curl --fail-with-body --request GET \
  --url https://api.apidot.ai/api/generate/status/task-unified-example \
  --header "Authorization: Bearer $APIDOT_API_KEY"
```

