# FFMPEG Guide

## Commands

### 1. H.264
`ffmpeg -i <INPUT_FILE> -c:v libx264 -crf <CRF> -c:a <AUDIO_ENCODER> -b:a <AUDIO_BITRATE> <OUTPUT_FILE>`

- **CRF**
   - `18` - transparent quality
   - `23` - good quality (default)
- **AUDIO_ENCODER**
  - `libopus` - best choice, only with _mkv_ container
  - `aac` - good choice, supported by _mkv_, _mp4_, _flv_
  - `copy` - copies audio stream
- **AUDIO_BITRATE**
  - `320k` - best quality (stereo)
  - `128k` - good quality (stereo)
  - _empty_ - if copying audio stream
- **OUTPUT_FILE**
  - `.mkv` - can be used with any audio codec
  - `.mp4` - can be used with _mp3_, _aac_, _ac3_, _flac_, _alac_ audio codecs

https://trac.ffmpeg.org/wiki/Encode/H.264

### 2. H.265
`ffmpeg -i <INPUT_FILE> -c:v libx265 -crf <CRF> -c:a <AUDIO_ENCODER> -b:a <AUDIO_BITRATE> <OUTPUT_FILE>`

- **CRF**
   - `18` - best quality
   - `23` - good quality
   - `28` - normal quality (default)
- **AUDIO_ENCODER**
  - `libopus` - best choice, only with _mkv_ container
  - `aac` - good choice, supported by _mkv_, _mp4_, _flv_
  - `copy` - copies audio stream
- **AUDIO_BITRATE**
  - `320k` - best quality (stereo)
  - `128k` - good quality (stereo)
  - _empty_ - if copying audio stream
- **OUTPUT_FILE**
  - `.mkv` - can be used with any audio codec
  - `.mp4` - can be used with _mp3_, _aac_, _ac3_, _flac_, _alac_ audio codecs

https://trac.ffmpeg.org/wiki/Encode/H.265

### 3. VP9
`ffmpeg -i <INPUT_FILE> -c:v libvpx-vp9 -crf <CRF> -b:v 0 -cpu-used <SPEED> -c:a <AUDIO_ENCODER> -b:a <AUDIO_BITRATE> <OUTPUT_FILE>`

- **CRF**
   - `31` - 1080p
   - `32` - 720p
   - `33` - 480p
   - `36` - 360p
   - `37` - 240p
   - `24` - 1440p
   - `15` - 2160p
- **SPEED**
  - `0` - best quality (default)
  - `2` - faster
  - `5` - best speed
- **AUDIO_ENCODER**
  - `libopus` - best choice
- **AUDIO_BITRATE**
  - `320k` - best quality (stereo)
  - `128k` - good quality (stereo)
  - _empty_ - if copying audio stream
- **OUTPUT_FILE**
  - `.webm` - best choice

https://trac.ffmpeg.org/wiki/Encode/VP9

## Comparison
Example:

| Codec              | Speed | Size/Bitrate |
|--------------------|-------|--------------|
| libvpx-vp9 -crf 31 | 4.3x  | 1x           |
| libx265 -crf 18    | 2.8x  | 2x           |
| libx264 -crf 18    | 1.3x  | 3x           |
| libx265 -crf 23    | 2.3x  | 1x           |
| libx264 -crf 23    | 1x    | 1.7x         |

## Other
- Change resolution: `-vf scale=<WIDTH>:<HEIGHT>` (if you want to save aspect ratio, use `-1` for one of dimensions)

## Notes
1. To calculate the bitrate to use for multi-channel audio: (_bitrate for stereo_) x (_channels_ / 2).
2. If using `libopus` with multi-channel audio (>stereo) should use `-af "channelmap=channel_layout=<LAYOUT>"`. For example, `-af "channelmap=channel_layout=5.1"`
3. https://trac.ffmpeg.org/wiki/Encode/HighQualityAudio
