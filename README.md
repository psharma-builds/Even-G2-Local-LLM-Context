# G2 Live Context

**Real-time conversation intelligence for Even Realities G2 smart glasses. Fully local pipeline - no cloud.**

An end-to-end system that listens to live conversation, transcribes it on local GPU, generates contextual insights with a local LLM, and mirrors them to the wearer's glasses display in near real time.

## What it does

You are in a meeting. Audio is captured from the phone microphone or a BLE wearable mic, streamed to a self-hosted GPU server, transcribed with faster-whisper, passed through a local LLM (Ollama) that produces short contextual insights - key points, open questions, names and commitments - and the output is pushed back to the phone as Android notifications, which the Even Hub app mirrors onto the G2 glasses display. Total loop: a few seconds, and no audio or text ever leaves the local network.

## Architecture

```
Phone mic / BLE wearable
   |
   v  (audio stream, local network)
GPU server (Ubuntu, CUDA)
   - faster-whisper  -> streaming transcription
   - pyannote        -> speaker segmentation
   - Ollama (Llama)  -> rolling context + insight generation
   |
   v  (Flask-SocketIO)
Android companion app (Flutter)
   |
   v  (notification mirroring via Even Hub)
Even Realities G2 display
```

## Design principles

- **Local-first:** every model runs on owned hardware; nothing transits a third-party API.
- **Latency-aware:** streaming transcription with incremental LLM context updates rather than batch processing.
- **Glanceable output:** insights are compressed to fit a heads-up display, not a document.

## Stack

Python (Flask-SocketIO, faster-whisper, pyannote) | Ollama / Llama | Flutter (Android companion) | CUDA (RTX GPU) | Even Hub SDK

## Status

**Deployed and in daily personal use.** Packaged for the Even Hub app store. Built and maintained as a personal project on self-hosted infrastructure.
