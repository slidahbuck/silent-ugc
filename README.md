# silent-ugc

A Codex and Claude Code skill for recreating silent TikTok or Instagram Reel app-ad hooks with your own AI actor and real app demo. Higgsfield is the default provider; Arcads remains available when explicitly requested.

The format is a short reaction shot with on-screen hook text, followed by a hard cut to an app screen recording. No dialogue or voiceover. This fork adapts [jasonlee-breadcrumb/silent-ugc](https://github.com/jasonlee-breadcrumb/silent-ugc) for Higgsfield.

## What it does

Provide a source video URL, actor image, and app demo. The skill will:

1. Retrieve the source and identify the hook/demo boundary.
2. Rebuild the hook frame without source text and place your actor in it.
3. Inspect the source performance and animate the actor using Higgsfield Genjutsu motion transfer, or a supported image-only route if identity drifts.
4. Burn approved hook text onto the generated hook and stitch it to your real demo.
5. Verify and deliver a persistent silent MP4.

Clone mode makes one video. Account mode creates multiple scenes with the same actor, hair, and app demo, varying rooms, outfits, lighting, and text. Text variations reuse completed hooks rather than paying for a new video generation each time.

## Requirements

- Codex or Claude Code with the **Higgsfield MCP** connected and sufficient generation allowance/credits.
- A generated or licensed actor image and a real app screen recording.
- A retrievable public TikTok/Instagram video URL, a direct media URL, or a source video file. Login-restricted or blocked sources may need a file upload.

Higgsfield uses its hosted sandbox for downloading, ffmpeg processing, text, and stitching. Local `ffmpeg` and `yt-dlp` are not required for that path. In clients with upload widgets, select local images/videos in the Higgsfield widget so the tools receive confirmed media IDs. Source and demo videos may need separate upload turns.

For the optional Arcads path, connect Arcads and install local `ffmpeg` and `yt-dlp` (`brew install ffmpeg yt-dlp`). Provider-specific instructions live under the skill's `references/` folder.

## Install in Codex

```bash
git clone --branch feat/higgsfield-provider https://github.com/slidahbuck/silent-ugc.git
mkdir -p ~/.codex/skills
cp -R silent-ugc/skills/silent-ugc ~/.codex/skills/
```

Use the skill on your next turn. If your Codex installation uses a custom `CODEX_HOME`, copy it to that directory's `skills/` folder instead.

## Install in Claude Code

Copy the same `skills/silent-ugc` folder into `~/.claude/skills/`, then restart the session if needed. Connect the chosen provider's MCP through that client's supported settings.

## Use

```text
Use $silent-ugc with Higgsfield to recreate this Instagram Reel hook:
[Instagram Reel URL]
Use my actor image and app demo. Make one video for Rentul focused on installs.
Show me the hook text, plan, and live credit estimate before generating.
```

For an account:

```text
Use $silent-ugc with Higgsfield to make 10 scenes from this TikTok hook:
[TikTok URL]
Keep the same actor and demo. Vary the rooms, outfits, lighting, and hook texts.
```

The skill collects missing assets, prepares the plan, and quotes costs before asking for generation approval. Merely installing or editing it does not start paid jobs.

## Cost

Costs depend on the current Higgsfield model, settings, duration, count, and allowance. The skill uses generation tools with `get_cost: true` before submission and quotes the full plan. It does not assume image passes or assembly are free. If unlimited/free-trial generations are requested, it preserves that choice and asks before any paid alternative.

## Actor identity and source rights

Use a generated or licensed actor and app footage you have rights to use. The skill recreates the source format and performance with your actor. Motion transfer can drift toward the source person's identity; inspect both stills and videos, and use image-only animation if needed rather than delivering a mismatched face.

## License

MIT. The original license is retained.
