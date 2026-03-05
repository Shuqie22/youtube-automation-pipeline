# YouTube True Crime Channel Automation

## What This Project Does

This workflow is a fully automated YouTube content pipeline that produces complete, ready-to-upload true crime videos from a single topic input. It handles everything from AI script generation to voiceover production, image animation, video assembly, caption burning, and YouTube upload without any manual editing work in between.

## The Problem It Solves

Running a YouTube channel manually is an enormous time commitment. Writing scripts, recording voiceovers, sourcing visuals, editing footage, adding captions, and uploading with proper SEO metadata can take 8 to 12 hours per video. This automation compresses the entire production pipeline into a single triggered workflow, making it possible to produce and publish multiple videos per week consistently.

## How It Works

The workflow begins with a script generation node powered by a large language model that receives a true crime case topic and produces a fully structured 20-scene video script in JSON format. Each scene contains a spoken transcript, an image prompt, and SEO metadata including tags, hashtags, and a viral title. From there the pipeline branches into parallel tracks. The image generation track takes each scene's image prompt and generates a photorealistic still image, then animates it with a subtle zoom motion to create cinematic movement. The voiceover track sends each scene's transcript to a text-to-speech API to produce a high-quality narration audio file. An ffmpeg node then merges each scene's animated image with its voiceover into a short video clip. Once all 20 scene clips are complete, another node concatenates them into a single full-length video, adds background music at a lower volume, burns animated captions using the word-highlight style, and compresses the final file. The workflow then automatically uploads the finished video to YouTube with the AI-generated title, description, tags, and hashtags. A custom thumbnail is generated and set on the video automatically as well.

## Tools Used

All core tools in this project run locally or use free and affordable API tiers.

- **n8n** (self-hosted, free) — the automation engine orchestrating the entire pipeline
- **OpenAI or compatible LLM API** — generates the 20-scene structured script and SEO metadata
- **ElevenLabs or compatible TTS API** — converts each scene transcript into a natural-sounding voiceover
- **NCA Toolkit** (self-hosted) — handles image generation, image animation, video concatenation, and caption burning
- **MinIO / S3** (self-hosted) — stores all intermediate files including images, audio clips, and scene videos
- **ffmpeg** (local) — merges audio and video for each scene, adds background music, and compresses the final output
- **YouTube Data API v3** — uploads the finished video and sets the thumbnail automatically

## Workflow Structure

The pipeline runs in this sequence. A trigger node starts the workflow with a case topic. The script generation node produces the full 20-scene JSON. A loop node iterates over each of the 20 scenes and for each one generates an image, animates it, generates a voiceover, gets the audio duration, and merges them into a scene video stored in S3. After the loop completes, a concatenation node joins all scene videos into one file. Background music is downloaded and mixed in. The video is compressed with ffmpeg. Captions are burned in with the word-highlight animation style. The final video is uploaded to YouTube with all metadata. A thumbnail is generated and set on the video. A Google Sheets row is appended to log the video title, upload ID, and YouTube URL for record-keeping.

## Screenshot

![Workflow Screenshot](https://github.com/user-attachments/assets/3b1d4112-5102-4dd4-897a-9f800ee9ff61)


## Credits

Built by Sukurah Yusuf in collaboration with Quyum Yusuf.

## How to Set It Up

To run this workflow yourself you will need n8n running self-hosted, an NCA Toolkit instance running locally or on a server, a MinIO or S3-compatible storage bucket, ffmpeg installed on the same machine as n8n, an ElevenLabs API key for voiceover generation, an LLM API key for script generation, a YouTube Data API v3 OAuth credential, and a Google Sheets spreadsheet for logging. Replace all placeholder values in the workflow JSON with your own credentials, file paths, and bucket names before importing.
