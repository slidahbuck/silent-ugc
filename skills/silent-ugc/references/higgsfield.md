# Higgsfield provider

Use the callable Higgsfield MCP tools in this session. Tool names below omit their server prefix. Current tool descriptions and returned schemas are the authority; do not copy Arcads arguments into Higgsfield calls. This path requires a connected Higgsfield account, not an Arcads connection or a local ffmpeg installation.

## Entry and tool discovery

- **Local/attached media without confirmed IDs:** call `media_upload_widget` immediately as the only tool in that turn. Resume when the widget returns IDs. Use `type: "auto", multiple: true` for images plus one video. If source and demo are both files, upload the second video separately and label each asset. Do not search local directories or use shell to ingest these user inputs.
- **Explicit Higgsfield slash preset:** resolve the exact token with `get_preset_instructions` before unrelated workflows, except a required exclusive upload turn. Follow a resolved recipe; never pass gallery IDs to ordinary generation tools.
- **Ordinary silent-UGC production:** call `get_workflow_instructions({})` to inspect current workflow routing. The silent actor-hook plus real demo format stays silent; do not substitute a narrated or continuous talking-head website ad. Load `video-editing` for trimming, text overlays, and assembly. Load its detailed resources only when native Higgsedit composition is needed.
- Use `models_explore({action: "get", model_id: ...})` for every model actually used. Inspect roles, allowed durations, aspect ratios, resolution, and audio settings. If a model is unavailable, use `action: "recommend"` with the intended inputs and task; obtain approval for material cost or output changes.
- Before the first generation without an explicit destination, call `get_preferences`. Create one project only if requested or `auto_create_project` is true; otherwise omit `folder_id`. Reuse any chosen destination for this conversation.

## Asset IDs and URLs

Generation inputs are `params.medias: [{role, value}]`, with confirmed **media IDs or completed generation job IDs** as values. Never put a local path, sandbox path, presigned upload URL, social page URL, or raw media URL in `value`.

For an HTTPS media-file URL, call `media_import_url({url, type})` first and retain its confirmed ID and URL. The import tool currently limits URL payloads to 50 MB; follow its live result for actual limits. A TikTok/Instagram page needs extraction before import and may require an uploaded file instead.

For files generated or downloaded inside the Higgsfield sandbox:

1. Call `media_upload` with an output filename and Content-Type **before** the sandbox operation.
2. In a single `sandbox_exec` command, obtain inputs, process the file, and PUT the bytes to the exact returned `upload_url` with the matching Content-Type. Require HTTP 200; preserve every presigned URL query parameter.
3. Call `media_confirm` only after the successful PUT. Use its confirmed ID and persistent URL afterward.

Reserve multiple outputs together when needed. Existing confirmed IDs can be reused across calls; Arcads' single-use reference-upload rule does not apply to Higgsfield media IDs.

## Source retrieval and analysis

Use `sandbox_exec` for Higgsfield downloading, ffmpeg/ffprobe, frame extraction, text processing, and concatenation. Do not use the client's local shell for this provider's media workflow. The hosted sandbox has ffmpeg, ffprobe, Python/Pillow, curl, and caption fonts. Check whether `yt-dlp` is available; if absent, install it in this disposable sandbox when supported. If unavailable, ask for a source upload rather than altering the user's local setup.

For a public TikTok or Instagram Reel/video page, run `yt-dlp` in the sandbox with the URL passed as a properly quoted argument. Use `--no-playlist` for one source, normalize the downloaded result to MP4 as needed, and upload it before the call exits. Retry a transient extraction error once. A login/private/unavailable error should lead to a source upload widget, not repeated retries or scraping private content.

Use `ffprobe` for source dimensions, duration, and frame rate. Use ffmpeg scene detection to find likely hook cuts, then inspect a contact sheet or short uploaded clip to confirm the cut. Export hook MP4, first-frame PNG, and contact sheet PNG to confirmed media before their sandbox expires. Never claim a frame was inspected if the client has no way to render it. If useful, `video_analysis_create({video_input_id: confirmedSourceId})` accepts uploaded videos; its URL alternative accepts YouTube links only, not Instagram or TikTok. Explain that short clips give more reliable analysis and poll `video_analysis_status` at its recommended interval.

## Model mapping

| Task | Tool and starting model | Inputs |
|---|---|---|
| Reconstruct the clean frame or put the actor in a scene | `generate_image`, `gpt_image_2_5` | Actor image, plus source first frame for close matching |
| Transfer the source performance | `generate_video`, `hf_mult_motion_control` (Genjutsu) | Actor scene image and exactly one driving hook video |
| Approximate performance without a driving video | `generate_video`, `seedance_2_5` when its live schema supports the settings, otherwise a recommended image-to-video model | Actor scene image only, plus the observed performance prompt |
| Trim, overlay hook copy, mute, and concatenate | `sandbox_exec` with ffmpeg, or current `video-editing`/Higgsedit workflow | Persistent input URLs and reserved output uploads |

The current generic generation tool describes Genjutsu roles as `image` and `video`; the catalog may expose `image_references` and `video_references`. Use the live model's declared roles and apply returned adjustments/coercions. There must be one actor scene image and exactly one hook video, with no demo as a motion reference. Do not use legacy `motion_control` or `ad-multiplier` as a substitute for a single Genjutsu motion transfer. An explicitly requested Ad Multiplier operation uses its own loaded workflow.

Request `aspect_ratio: "9:16"` only when supported. If the motion model has no aspect-ratio parameter, prepare portrait reference media and verify the output dimensions rather than inventing unsupported fields. The current Genjutsu catalog exposes resolution but no explicit duration/audio controls; use the trimmed reference video's timing and remove audio in final assembly. Re-check on each run.

For the current Seedance image-only fallback, use `mode: "omni_reference"`, the actor scene as `image_references`, `aspect_ratio: "9:16"`, and `generate_audio: false`. Its default mode is text-only and its default audio is on, so set both explicitly. Its current minimum duration is four seconds; generate at least that duration and trim a shorter hook afterward. Do not include the source video in this identity-preserving fallback. Re-verify these fields in the live catalog before quoting or submitting.

## Credits and submission

Use `generate_image` or `generate_video` with the intended `params` plus `get_cost: true` for a cost-only preflight; this submits no job. Quote the cost of every image pass and video scene, multiplied by the approved count, and any separately quoted editing charges. For future generated references, establish required settings and use an equivalent reference only when the tool accepts it for quoting; disclose provisional estimates and re-quote with the actual reference before submission. Do not fabricate IDs to make a preflight pass. Remove `get_cost` when submitting the authorized generation.

Do not carry over Arcads' fixed credit prices or claims of free image passes. Additional text variants can avoid regeneration, but rendering/export costs depend on live tools. If a result asks for a balance choice (`unlim_choice`), present it and resume with the user's selected `use_unlim` value. If the user explicitly requests unlimited/free-trial generations, preserve `use_unlim: true` on every generation and retry; a rejected request does not authorize charging credits. Follow returned recovery tools and obtain approval for a paid alternative.

Generation returns job IDs. Use `jobs_wait` with `timeout_seconds: 15`, obey `poll_after_seconds`, and stop on terminal failure. A timeout may leave an unknown submission outcome: recover known job IDs/status before retrying. Never blindly resubmit a whole batch.

Use `generate_image_batch`/`generate_video_batch` for independent scene prompts, with stable indices, `count: 1`, and approved inputs/settings. Preflight with the single-generation tools because batch submission is not a cost-only quote. Submit batches within the live limits and wait in groups supported by `jobs_wait` (at most eight is a conservative group size). After the whole set is terminal, call `show_generation_by_ids` once with the complete indexed set. For a single direct generation, use its automatically updating widget; do not add a duplicate result display unless requested.

## Assembly and durable delivery

Before editing, load `get_workflow_instructions({workflow: "video-editing"})`. For ordinary trims, static hook text, muting, and joining, ffmpeg in `sandbox_exec` is sufficient. Use native Higgsedit and its live workflow references for animated/native compositions when requested.

The sandbox is ephemeral, typically discarded about ten seconds after a foreground call finishes. Do not depend on a file surviving separate calls. Reserve outputs first, then combine input download, processing, verification, and output PUT in one sandbox command. Use `background: true` and poll returned log/status paths for long work, keeping the upload in that same background job. Call `media_confirm` after its successful PUT. Preserve intermediate IDs/URLs instead of relying on an `assets/` sandbox directory.

For ffmpeg assembly:

- Re-encode the hook trim for frame accuracy. Strip audio from both segments.
- Fit/pad to a common portrait canvas with square pixels and one frame rate; choose dimensions supported by the generated hook. Do not upscale just to imply extra detail. Preserve legible demo UI.
- Burn wrapped white text with a black outline over the hook only, using a sandbox font. Pass copy via a UTF-8 text file or correctly escaped filter arguments so punctuation cannot change the command. Keep text inside safe areas.
- Concatenate normalized segments with a hard cut. Export H.264, yuv420p, `-an`, and `-movflags +faststart` to an MP4.
- Probe the final output for expected duration/dimensions and zero audio streams. Inspect frames around the cut and text before delivery. Keep confirmed URLs for any previews needed after the sandbox call.

Return the confirmed final MP4 URL with a playable preview when supported. State any inspection limitation. A transient URL, local path, sandbox path, or generation ID alone is not a finished deliverable. Record scene indices, source/actor/demo IDs, completed job IDs, cut times, hook copy, and final confirmed IDs/URLs in the task's existing notes or requested manifest.
