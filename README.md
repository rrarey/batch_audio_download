# Batch Audio Download

**Rich Rarey**  
Created May 17, 2026 · Updated July 13, 2026

`batch_audio_download.sh` is a macOS/Linux command-line utility that downloads audio from online videos using `yt-dlp` and converts it to a configurable audio format with FFmpeg.

It accepts either a small number of URLs directly on the command line or a text file containing many URLs. Each URL is processed separately, with progress reporting and a final success/failure summary.

The utility was originally created to replace a Parallels Toolbox workflow that downloads only one file at a time, but it is generic and can be used with sites supported by `yt-dlp`.

## Features

- Downloads audio from one or many online videos
- Accepts URLs directly or from a text file
- Supports configurable output formats such as WAV, MP3, FLAC, M4A, and Opus
- Shows progress for each URL
- Continues processing if an individual download fails
- Reports the number of successful and failed downloads
- Creates the output directory automatically
- Ignores blank lines and comment lines in URL-list files

## Requirements

The script requires:

- [`yt-dlp`](https://github.com/yt-dlp/yt-dlp) to download media
- [FFmpeg](https://ffmpeg.org/) to extract and convert audio

On macOS, install both with Homebrew:

```bash
brew install yt-dlp ffmpeg
```

Verify that they are available:

```bash
yt-dlp --version
ffmpeg -version
```

## Make the Script Executable

After downloading or cloning the repository, open Terminal and change to the directory containing the script:

```bash
cd /path/to/batch-audio-download
```

Give the script execute permission:

```bash
chmod +x batch_audio_download.sh
```

You only need to do this once. The script can then be run directly with `./batch_audio_download.sh`.

## Configuration

Edit the configuration values near the top of `batch_audio_download.sh`:

```bash
OUTPUT_DIR="./downloaded-stuff/my-files"
AUDIO_FORMAT="wav"
```

`OUTPUT_DIR` specifies where downloaded audio files are saved. The directory is created automatically if it does not exist.

`AUDIO_FORMAT` specifies the output format passed to `yt-dlp` and FFmpeg. Common choices include:

```bash
AUDIO_FORMAT="wav"
AUDIO_FORMAT="mp3"
AUDIO_FORMAT="flac"
AUDIO_FORMAT="m4a"
AUDIO_FORMAT="opus"
```

Only one `AUDIO_FORMAT` assignment should be active at a time.

## Usage

### Download one video

```bash
./batch_audio_download.sh \
    "https://somesite.org/video/videoname-here/"
```

### Download several videos

```bash
./batch_audio_download.sh \
    "https://example.com/video-one" \
    "https://example.com/video-two" \
    "https://example.com/video-three"
```

### Download URLs from a file

Create a text file such as `urls.txt`, with one URL per line:

```text
# Videos to download
https://example.com/video-one
https://example.com/video-two
https://example.com/video-three
```

Blank lines and lines beginning with `#` are ignored.

Run the batch:

```bash
./batch_audio_download.sh urls.txt
```

## Example Output

```text
Reading URLs from: urls.txt
Saving wav audio files to: ./EP516-525/ORIGINALS
URLs to process: 3

----------------------------------------
[1/3] Processing:
https://example.com/video-one

... yt-dlp output ...

[1/3] Complete: https://example.com/video-one

========================================
Batch processing complete.
Succeeded: 3
Failed:    0
Total:     3
```

If a download fails, the script records the failure, continues with the remaining URLs, and exits with a nonzero status after the batch is complete.

## URL-List File Notes

- Use one URL per line.
- Blank lines are ignored.
- Lines beginning with `#`, including comments preceded by spaces, are ignored.
- Windows-style carriage returns are removed automatically.

## Output Filenames

Output filenames are based on the online video's title and use the selected audio format's extension:

```text
-video-title.wav
-video-title.mp3
-video-title.flac
```

## Supported Sites

The utility can work with sites supported by `yt-dlp`. Site behavior may change over time, and some sites may require authentication, cookies, or additional `yt-dlp` options.

Use this utility only for content you have permission to download and use.
