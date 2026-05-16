# Beat Sync Music Video Tutorial

> Auto-sync any video clips to your beats using AI BPM detection. No editing skills needed.

Used by 10,000+ music producers. Full working Python code included.

## What This Does

Drop any audio file → AI detects BPM → clips auto-sync to every beat, drop, and transition. GPU shaders add glitch, glow, chromatic aberration in real time. Export 4K.

**Used in production:** [BeatSync PRO](https://beatsyncpro.ai) — 40,000 free clips included.

---

## Quick Start

```python
pip install librosa moviepy numpy
```

```python
import librosa
import numpy as np

def detect_beats(audio_file: str) -> tuple[float, np.ndarray]:
    """Returns (bpm, beat_times_in_seconds)."""
    y, sr = librosa.load(audio_file)
    tempo, beat_frames = librosa.beat.beat_track(y=y, sr=sr)
    beat_times = librosa.frames_to_time(beat_frames, sr=sr)
    return float(tempo), beat_times

bpm, beats = detect_beats("your_track.mp3")
print(f"BPM: {bpm:.1f} | Beats: {len(beats)}")
```

```python
from moviepy.editor import VideoFileClip, concatenate_videoclips

def sync_clips_to_beats(clips: list[str], beat_times: np.ndarray) -> str:
    """Cut and arrange clips so each cut lands on a beat."""
    segments = []
    clip_duration = beat_times[1] - beat_times[0] if len(beat_times) > 1 else 1.0

    for i, beat_time in enumerate(beat_times[:-1]):
        clip_file = clips[i % len(clips)]
        duration = beat_times[i + 1] - beat_time
        clip = VideoFileClip(clip_file).subclip(0, min(duration, VideoFileClip(clip_file).duration))
        segments.append(clip)

    final = concatenate_videoclips(segments)
    out = "synced_output.mp4"
    final.write_videofile(out, fps=30, codec="libx264")
    return out
```

---

## GPU Shaders (Real-Time)

```python
# Glitch effect on drops — triggers when beat intensity > threshold
def apply_glitch_on_drop(frame: np.ndarray, intensity: float) -> np.ndarray:
    if intensity > 0.8:
        shift = np.random.randint(5, 20)
        frame = np.roll(frame, shift, axis=1)
        frame[:, :shift] = 0
    return frame
```

---

## Free Clips Library

40,000+ royalty-free music video clips — no watermark, no login:

- **Abstract/Particle**: laser grids, energy fields, glitch loops
- **Urban/City**: streets, neon, underground
- **Nature/Cinematic**: slow-motion fire, water, smoke
- **VHS/Retro**: grain, scanlines, tape artifacts

**Download free:** [beatsyncpro.ai](https://beatsyncpro.ai)

---

## Full Production Tool

Manual Python works for experiments. For production (40K clips, GPU shaders, 4K export, one click):

→ **[BeatSync PRO](https://beatsyncpro.ai)** — the tool producers actually use.

---

## Resources

- [librosa docs](https://librosa.org/doc/latest/)
- [moviepy docs](https://zulko.github.io/moviepy/)
- [BeatSync PRO (free tier)](https://beatsyncpro.ai)
- [40,000 Free Clips](https://beatsyncpro.ai/free-clip-packs.html)
