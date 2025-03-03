# Besage Chat - CDN

A Cloudflare Worker that serves as an image CDN for a real-time chat application, providing secure storage and fast delivery of images using Cloudflare R2.

## Overview

This worker handles uploading, retrieving, and deleting images from a Cloudflare R2 bucket. It implements basic authentication for write operations while allowing public read access to stored images.

## Features

- Image Upload: Store images via PUT requests
- Image Retrieval: Fetch images via GET requests
- Image Deletion: Remove images via DELETE requests
- Authentication: Secure endpoints with pre-shared key authentication
- Fast Delivery: Leverage Cloudflare's global network for image delivery

## Prerequisites

- Node.js (v16 or higher)
- A Cloudflare account
- Cloudflare Workers subscription
- Cloudflare R2 bucket setup

## Setup and Installation

1. Clone the repository

```bash
git clone git@github.com:Djkde01/realtime-chat-cdn.git
cd r2-worker
```

2. Install dependencies

```bash
npm install
```

3. Configure environment
   Create a `.dev.vars` file for local development:

```bash
AUTH_KEY_SECRET=your_secret_key_here
```

4. Update the R2 bucket reference
   **Note:** There's a mismatch between the bucket binding in `wrangler.jsonc` (IMG_CDN) and the code in `index.js` (MY_BUCKET). You need to update one of them to match.

Either change the binding in wrangler.jsonc to:

```JSON
"r2_buckets": [
  {
    "binding": "MY_BUCKET",
    "bucket_name": "besage-chat-imgs"
  }
]
```

Or update the code in `index.js` to use `env.IMG_CDN` instead of `env.MY_BUCKET`. 5. Set up your secret in Cloudflare

## Development

Run the worker locally:

```bash
npm run dev
```

## Testing

Run tests with Vitest:

```bash
npm test

```

## Deployment

Deploy to Cloudflare Workers:

```bash
npm run deploy
```

## API Usage

### Uploading an image

```bash
curl -X PUT \
  -H "X-Custom-Auth-Key: your_secret_key" \
  --data-binary @image.jpg \
  https://your-worker-url.workers.dev/image.jpg
```

### Retrieving an image

```bash
curl https://your-worker-url.workers.dev/image.jpg

```

### Deleting an image

```bash
curl -X DELETE \
  -H "X-Custom-Auth-Key: your_secret_key" \
  https://your-worker-url.workers.dev/image.jpg

```

### Authentication

- PUT and DELETE requests require the `X-Custom-Auth-Key` header with your secret
- GET requests are public and require no authentication

## License

MIT License
