+++
title = "SimpleAUHost: a lightweight live Audio Unit host for macOS"
date = "2026-07-26"
author = "staubichsauger"
description = "Introducing SimpleAUHost, a focused macOS app for routing live audio through Audio Unit effects without opening a full DAW."
+++

Sometimes a full DAW is more than a live setup needs. I wanted a small, focused
tool that could take audio from an interface, run it through Audio Unit effects,
and send it back out with predictable routing. That became
[SimpleAUHost](https://github.com/staubichsauger/simple-au-host).

SimpleAUHost is a native macOS live audio rack. It uses Core Audio directly,
hosts installed Audio Unit effects, and keeps the workflow centered on tracks,
routing, and plugin chains.

## What it can do

- Run multiple mono or stereo tracks through one audio interface
- Route each track from selected hardware inputs to selected outputs
- Build per-track Audio Unit insert chains
- Choose realtime, buffered, or broadcast processing per track
- Open embedded plugin editors
- Save complete sessions, reusable chain presets, and parameter presets
- Show diagnostics for dropouts, callback timing, buffers, and worker load
- Control tune workflows locally from Bitfocus Companion

Enabled tracks get exclusive output channels, so conflicting routes are caught
before the audio engine starts. Sessions and presets live in `~/Music/SAH`,
making setups easy to find and back up.

## Built for live use

The app talks to the selected device through AUHAL rather than hiding the
hardware behind a larger production environment. Realtime tracks run on the
hardware callback cadence, while buffered and broadcast tracks can move heavier
work to background workers.

There is also a local control API for show-control workflows. The included
Bitfocus Companion module supports tune on/off, staged key changes, panic, and
next/previous song actions.

## Try it

SimpleAUHost requires macOS 14 Sonoma or newer and an interface that exposes the
inputs and outputs you want to use. The app is currently unsigned, so macOS may
ask you to approve it in Privacy & Security on first launch.

Download the current build and the optional Companion module from the
[latest release](https://github.com/staubichsauger/simple-au-host/releases/latest),
or browse the
[source and documentation](https://github.com/staubichsauger/simple-au-host).
