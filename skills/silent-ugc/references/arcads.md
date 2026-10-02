# Arcads provider (only when explicitly selected)

Retain the Arcads path for users who choose it. Do not require these tools or account setup when using Higgsfield. Use current tool schemas and live credit estimates rather than old fixed prices.

## Intake and connection

Call `arcads_list_products` to verify access and note the product ID. If no Arcads tools are callable, ask the user to connect their Arcads MCP using the connection details from their dashboard; do not invent an endpoint. If the user prefers Higgsfield instead, switch only with their authorization.

For this provider's local workflow, locate actor and demo files in the working folder when available. Use `yt-dlp` for public TikTok or Instagram Reel/video source URLs and ffmpeg for splitting/extracting frames. Use one transient retry; request a source file for login/private/unavailable errors. Preserve intermediates in the working assets folder when requested.

Arcads reads uploaded files rather than local paths. Call `arcads_get_upload_url`, PUT the bytes with the matching Content-Type, and use its returned `filePath`. Presigned upload URLs are single use; follow the current tool's reference reuse requirements and copy query parameters exactly.

## Images and motion

For a new scene, use `arcads_generate_image_nano_banana` with a currently supported nano-banana model and the actor reference. For close source matching, recreate the first frame with all text removed, then use `arcads_generate_image_gpt` with the clean frame and actor photo to replace the person. Inspect face identity before video generation; image retries are not assumed free.

For motion transfer, verify that `arcads_generate_video_seedance_25` supports the actor scene image and hook video references. Use its declared image/video parameters, portrait settings, supported duration, and audio controls. Do not assume a reference clip always preserves the actor's face. Inspect the result and use a supported image-only animation route with the observed performance prompt if identity drifts, within the approved budget.

## Trim, text, stitch, and delivery

Trim to the source hook length. Use `arcads_add_text_overlay` with supported TikTok-style text options, or local ffmpeg for deterministic text variants. Use `arcads_stitch_videos` with [texted hook, real demo], ensuring matching dimensions and no audio. Arcads-specific style strings such as `tiktok-none` and `tiktok-dark` are not Higgsfield parameters.

Poll `arcads_get_asset` using the tool's recommended interval rather than tight loops. Deliver the completed MP4 and retain requested intermediates. Check the actor, hook timing, readable text, demo, final duration, and absence of audio. Quote current provider costs for images, video, editing, and retries before spending; do not reuse the original README's conflicting credit totals.
