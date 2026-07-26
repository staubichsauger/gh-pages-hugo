+++
title = "SimpleAUHost: live tuning without the pro-audio workflow"
date = "2026-07-26"
author = "staubichsauger"
description = "SimpleAUHost lets an expert prepare live tuning and broadcast processing, then gives volunteer operators a focused workflow with direct Stream Deck control."
+++

Live tuning is usually built with professional audio tools. Those tools are
powerful, but they also expose a lot of complexity that should not have to be
managed by every person operating a live event.

I wanted an expert to be able to prepare the audio routing, tuning plugins,
presets, processing chains, and Stream Deck controls ahead of time—and then hand
the finished technical setup to volunteers who should not need to learn a DAW
or navigate a professional live-plugin host during the show.

The operators can still handle the parts that naturally change from week to
week, such as building the setlist and assigning a key to each song.

That is why I created
[SimpleAUHost](https://github.com/staubichsauger/simple-au-host).

SimpleAUHost is a native macOS Audio Unit host designed around prepared live
shows. It separates the work of configuring a show from the smaller set of
controls needed to operate it reliably.

![SimpleAUHost Perform view with two tuning tracks, show management, an editable three-song setlist, and centralized key controls](/img/simple-au-host/perform.jpg)

*The Perform view combines tuning strength, show loading, setlist navigation,
key staging, and apply controls.*

## Low-latency tuning and broadcast processing together

Live tuning needs low latency. A broadcast or livestream mastering chain can be
much heavier, but its latency is usually less critical.

Running both through one conventional processing buffer creates an unnecessary
compromise: increasing the buffer to keep the mastering chain stable also makes
the tuning path slower, while keeping everything at a very small buffer risks
dropouts in the heavier chain.

SimpleAUHost solves this with per-track latency classes:

- **Realtime** processes latency-sensitive tracks on the hardware callback
  cadence.
- **Buffered** gives more demanding tracks larger internal processing blocks.
- **Broadcast/Post** provides the largest blocks and safety preroll for
  non-critical broadcast and mastering paths.

This means a realtime vocal tuning path and a heavy livestream mastering chain
can run alongside each other without forcing both tracks to use the same
latency tradeoff.

![SimpleAUHost latency setup with separate realtime, buffered, and broadcast processing block sizes](/img/simple-au-host/latency-setup.jpg)

*Each processing path gets its own latency and stability tradeoff instead of
forcing every track to share the same buffer size.*

## Technical setup by an expert, weekly operation by volunteers

A complete show can contain its device configuration, tracks, channel routing,
latency classes, plugin chains, and tuning setlist. Chain and parameter presets
make known-good technical configurations reusable.

An audio expert can prepare the routing and processing once. Operators can then
load that setup and handle the parts that change for each event—especially the
song list and musical keys—without rebuilding the audio system or opening the
individual plugin interfaces.

This is particularly useful for churches and similar organizations where
different volunteers operate the system and the setlist changes every week.
Adding, naming, ordering, duplicating, and assigning keys to songs is kept
simple, while the underlying routing and processing remain prepared and
consistent.

For dedicated installations, SimpleAUHost can automatically load either the
last saved show or a specific show when it launches. It can open directly on the
Perform tab and start the audio engine automatically, reducing startup to
opening the app.

A specific startup show can also be loaded as a template. The expert-prepared
routing and processing remain untouched, while an operator can create the
current week's setlist and save the result as a separate, dated show. This makes
one trusted technical configuration reusable across recurring events.

The goal is not to remove advanced configuration. It is to let that
configuration happen once, in advance, instead of asking every volunteer to
understand and reproduce it during a live event.

## A setlist instead of scenes and snapshots

Tuning changes should follow the setlist, not force the operator to manage an
abstraction built for a different purpose.

SimpleAUHost includes an editable song list in which each song has its own name,
notes, and musical key. An operator can quickly build and order the setlist for
the current event, even if a different volunteer performs that task each week.
During the show, moving to the next or previous song applies its key to the
supported tuning plugins.

The song list is a straightforward alternative to building a separate scene or
snapshot for every song and every possible tuning state. It keeps the weekly
setlist and its keys inside the show while leaving the expert-prepared audio
configuration intact.

Keys can still be staged and applied manually when the setlist changes or the
operator needs to deviate from the prepared order.

## Centralized tuning control

SimpleAUHost provides dedicated parameter-level integration with
[Waves Tune Real-Time](https://www.waves.com/plugins/waves-tune-real-time).

When supported tuning plugins are loaded on tracks, the Perform view brings
their essential live controls into one place. Operators can manage tuning
on/off, musical key and scale, tuning strength, the current song, and the
prepared setlist without opening each plugin interface.

SimpleAUHost maps those controls directly to the relevant Waves Tune Real-Time
parameters, including bypass, Scale Root, Scale Type, Tune Speed, and Note
Transition. Selecting a song, applying a staged key, or changing the tuning
strength updates the recognized plugin instances directly instead of recalling
generic plugin snapshots.

Support for
[Simple Live Tune](https://github.com/staubichsauger/simple-live-tune) is also
built into this centralized workflow. SimpleAUHost maps the same operator
controls to its bypass, key, scale, retune-speed, and note-transition
parameters. Simple Live Tune is still in development and has not been released
yet, so this integration is currently available for development and testing
rather than as a downloadable end-user plugin.

## First-class Stream Deck control

The centralized tuning state is also available through SimpleAUHost's local
control API and dedicated
[Bitfocus Companion](https://bitfocus.io/companion) module. This makes Stream
Deck an optional extension of the Perform workflow: the controls in the app and
on the control surface operate the same tuning state, setlist, and supported
plugin instances. Available operations include:

- Tune on, off, and toggle
- Staging and applying a musical key
- Separate note, accidental, and scale controls
- Panic
- Next and previous song
- Feedback for the active and staged keys, engine state, song name, and setlist
  position

A Stream Deck can therefore show what is currently active and what comes next,
while giving a volunteer a focused set of controls for operating the show.

## What else it can do

- Run multiple mono or stereo tracks through one audio interface
- Route each track from selected hardware inputs to selected outputs
- Host installed Audio Unit effects in per-track insert chains
- Open embedded plugin editors
- Save sessions, chain presets, and parameter presets
- Show diagnostics for dropouts, callback timing, buffers, and worker load

SimpleAUHost requires macOS 14 Sonoma or newer. The app is currently unsigned,
so macOS may ask you to approve it in Privacy & Security on first launch.

Download the current app and Companion module from the
[latest release](https://github.com/staubichsauger/simple-au-host/releases/latest),
or browse the
[source and documentation](https://github.com/staubichsauger/simple-au-host).

Waves and Waves Tune Real-Time are trademarks of Waves Audio Ltd. SimpleAUHost
is an independent project and is not affiliated with, sponsored by, or endorsed
by Waves Audio Ltd.
