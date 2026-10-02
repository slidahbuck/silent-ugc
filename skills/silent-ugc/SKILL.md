---
name: silent-ugc
description: Recreate silent TikTok or Instagram Reel app-ad hooks with your own AI actor, on-screen text, and real app demo using Higgsfield by default or Arcads when requested. Use for one hook or a consistent actor account with multiple scenes and text variations.
---

# Silent UGC: actor hook plus app demo

Recreate the source video's framing, pacing, and subtle performance with the user's own actor. Deliver a vertical MP4: a silent reaction hook, readable on-screen text, then a hard cut to a real app screen recording. Use the real demo rather than generating app UI. Support TikTok URLs, Instagram Reel/video URLs, direct media URLs, and uploaded videos. A URL is usable only if the media can be retrieved.

## Provider and intake

Use **Higgsfield by default**. Read [references/higgsfield.md](references/higgsfield.md) for its tool mapping, uploads, model discovery, cost preflight, rendering, and delivery. Do not require Arcads for this path. If the user explicitly chooses Arcads, read only [references/arcads.md](references/arcads.md) for provider mechanics. Do not silently change providers if authentication, credits, or tools are unavailable.

For Higgsfield in a client with upload widgets, when the user refers to local or attached media without confirmed media IDs, call `media_upload_widget` immediately as the sole tool in that turn, then resume after upload. Do not search local folders or use shell tools to access those user inputs. `type: "auto", multiple: true` accepts actor images plus one video; collect a second video in a separate upload turn if both source and demo are files.

Use information already supplied and ask only for missing inputs:

- **Mode and count:** one recreated hook (clone mode), or multiple scenes with one consistent actor (account mode). Use the requested count; if absent, establish it before quoting a batch.
- **App and goal:** app/store/site link or description, target audience, and installs, trials, or awareness. Read the supplied page for features and verify claims used in the copy.
- **Source:** one TikTok or Instagram Reel/video URL, direct video URL, or uploaded video. For a profile or multi-item post, identify the intended video before generation.
- **Actor:** a generated or licensed adult actor image, frontal, well lit, ideally at least 700px. Keep the face and hair consistent across scenes.
- **Demo:** a real screen recording of the app. Remove any existing hook and mute the recording for this silent format.

## Plan and approval

Prepare the source cut, performance description, hook text, model settings, and live cost estimate before requesting generation approval. A request to install or modify this skill does not authorize media generation.

Suggest short, casual hook texts, normally under 12 words. Adapt angles to supported app features: a real price comparison, common mistake, beginner frustration, saved effort, or a demonstrated result. Avoid invented personal experience, prices, or outcomes. For account mode, show the full scene and text list, actor, count, and estimated total. Obtain explicit approval before generating images or videos unless this exact plan and budget are already authorized. Paid retries must remain inside that budget; ask before exceeding it.

Retexting reuses an existing hook and avoids a new video generation. Do not call image generation, editing, stitching, or exports free unless the current provider confirms it.

## Per-video workflow

### 1. Retrieve and split the source

Follow the selected provider reference to retrieve the source and persist it before temporary storage expires. Social post pages are not direct video files. For public TikTok and Instagram URLs, use a supported downloader; if retrieval requires login or fails after a bounded retry, use the provider's upload surface for a user-supplied source file. Do not promise every Instagram post will download.

Find the hook-to-demo boundary using scene-change scores and visual inspection. Keep only the hook, usually two to four seconds; the user's demo replaces the source demo. Mute both segments. Extract a clean first frame and representative frames across the hook. Inspect timing, gaze, hand position, expression intensity, head movement, and handheld drift. Describe what stays still as well as what moves; subtle eye movement is often enough. If the frames cannot be inspected, disclose the limitation rather than claiming frame-by-frame analysis.

### 2. Build the actor scene image

Generate the actor directly in a new room for scene variations. To match a source closely, first reconstruct the frame with all text removed, then replace its person with the user's actor, preserving the pose, framing, room, outfit, and light. A reference-based image editor can perform these passes; verify the selected model supports the supplied references.

Example brief:

> Use the actor reference for the person's exact face and hair. Keep the source frame's vertical selfie framing, pose, hand position, room, outfit, and uneven lighting. Remove all source text. Preserve real skin texture, mild sensor noise, soft compression, and an amateur front-camera look. No beautification or retouching.

Inspect the generated still before animating. If the face resembles the source creator or text remains, correct it within the approved budget. Identity must be checked again in the generated video.

### 3. Animate the hook

Prefer verified motion transfer when the selected provider supports an actor scene image plus a driving hook video. Higgsfield uses Genjutsu through `generate_video`, not Arcads-specific functions. Motion transfer can leak the source identity or exaggerate gestures, so inspect the result rather than promising exact face preservation.

Example brief:

> Keep the reference image's actor, exact face and hair, room, outfit, and light. Transfer the driving clip's gaze, hand position and movement, expression timing, head motion, and handheld camera drift. Keep the restrained performance and raw front-camera quality. No on-screen text or dialogue.

If identity drifts, use a supported image-to-video model with only the actor scene image and a prompt derived from the observed performance, within the approved plan and budget. Explain that this approximates the movement rather than transferring it exactly. Respect live model duration and resolution constraints; generate a supported duration and trim to the source hook length when needed. Disable generated audio if supported and remove all audio during final assembly regardless.

### 4. Trim, text, and assemble

Trim the generated hook to the selected cut. Add white text with a black outline, two or three lines, matching the source placement while remaining inside platform safe areas. Burn text only onto the hook, then cut directly to the user's demo.

Normalize both segments to matching dimensions, frame rate, and encoding before concatenation. Preserve readable app UI: fit or pad the demo instead of blindly cropping it. Default to 9:16, H.264 MP4, and no audio track. Check the final duration, hook/demo boundary, actor identity, text readability, demo content, and actual absence of audio. Deliver a persistent playable/downloadable result, not an unexported sandbox path.

## Account variations

Reuse the same approved actor and demo. Vary rooms, outfits, lighting, and hook copy while retaining the actor's face and hair: bedroom lamp, daylight hoodie, kitchen counter, parked car, couch, or outdoor cafe. Keep gestures consistent with the driving clip; use another reference or the image-only route when a different gesture is required. Do not ask one driving gesture to transfer an incompatible performance.

Generate a small initial set and check identity and restraint before scaling. Batch independent scenes with the selected provider's batch tools after their image references are ready. Text rotations reuse completed hooks. Track each scene's source, actor image, generation IDs, copy, cut time, settings, cost quote, and final media ID/URL so work can resume without regenerating completed clips.
