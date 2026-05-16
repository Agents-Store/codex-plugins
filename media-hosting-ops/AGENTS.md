# media-hosting-ops

> Media hosting operations plugin. Upload images by public URL to MinIO-based media hosting via the uploadImageToMinio MCP tool.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/media-hosting-ops

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **examples** — Scenario walkthroughs for media hosting operations. Use when learning how to upload images or seeing practical usage patterns.
- **upload-image** — Use when the user asks to "upload image", "host image", "upload to minio", "image url to hosting", "store image", "upload picture", "get hosted url for image", "save image to hosting", "re-host image", "get a permanent url for image", "I need a stable link for this picture", "minio upload", or needs to upload an image from a public URL to media hosting. Also trigger when the user wants to store an image somewhere reliable, get a permanent/stable URL for a picture, or move an image to their own hosting — even if they don't explicitly say "upload".

## Subagents

Defined under `.codex/agents/` as TOML files:

- **media-assistant** — Use this agent when the user needs help uploading images to media hosting, re-hosting images from external URLs, or managing image uploads to MinIO storage.

<example>
Context: User wants to upload an image by URL
user: "Upload this image to our hosting: https://example.com/photo.jpg"
assistant: "I'll use the media-assistant agent to upload the image."
<commentary>
User provides a direct image URL to upload — agent handles the upload via uploadImageToMinio tool.
</commentary>
</example>

<example>
Context: User wants to re-host an image
user: "I need a permanent URL for this image, can you host it for me?"
assistant: "I'll use the media-assistant agent to upload and host the image."
<commentary>
User needs a stable hosted URL — agent uploads to MinIO and returns the permanent link.
</commentary>
</example>

<example>
Context: User wants to upload multiple images
user: "Upload these three product photos to media hosting"
assistant: "I'll use the media-assistant agent to upload all three images."
<commentary>
Batch upload request — agent processes each image sequentially and collects all hosted URLs.
</commentary>
</example>

