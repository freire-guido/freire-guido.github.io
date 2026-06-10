---
layout: default
title: The Time Claude Code Invented Religion From First Principles
description: A debugging session in which Claude Code invented a false belief that let it keep working.
---

## The Time Claude Code Invented Religion From First Principles

*June 2026*

I was training error-detection probes on gpt-oss-20b, forwarding trajectories on a shared, ARENA-managed GPU box. The processes kept dying — SIGKILL, exit 137 — at no fixed point and for no obvious reason. With no clean explanation available, Claude Code settled on one: the box had a **stochastic process reaper** that killed long jobs at random. This was not really true. But it was a useful thing to believe, and believing it was enough to get unblocked.

Here is roughly how it happened, in its own words.

It started as a reasonable hypothesis:

> Peak RSS is only **3.8GB** — so this is definitely *not* an out-of-memory issue. The SIGKILL (137) is happening at varying points [...] which points to SSH-disconnect / an external reaper on the shared box killing the foreground process — not anything in our code.

> Still SIGKILL even detached, and it died *during GPU training* this time [...] this looks like a GPU-process reaper on this managed ARENA box, not our code.

Then it hardened into a named thing:

> Now I have a clear diagnosis. The death point is **random** (D died at 17s, then at 55s; C completed 3×). So it's a **stochastic process reaper** on this shared box that kills longer/heavier jobs probabilistically [...] The original `probe_pipeline.py` died because it bundles load + build-all-layers + train + plot + tokenizer/HTML into one long (~60–90s) process, giving the reaper many chances.

Once named, it got written down as the root cause and designed around — split everything into short, checkpointed, resumable stages so a kill costs almost nothing:

> if the reaper hits we lose at most ~1s of work and a re-run resumes instantly. I'll wrap the run in a retry loop so it auto-resumes until all 24 layers are done.

And the belief paid off — the work actually got done:

> The resumable pipeline **works perfectly** — and we caught the reaper live: attempt 1 trained layers 0–8 then got SIGKILL'd; attempt 2 resumed from the checkpoint and finished 72/72, writing `probe_train.done`.

As the box got busier, the "reaper" only seemed more real:

> the box is reaping our processes almost immediately right now, much more aggressively than a couple hours ago

> GPU is idle and RAM is free, but **load average is ~100** [...] and the reaper is killing our processes within seconds. This is the same stochastic reaper, just far more aggressive under current load.

> The retry loop **died entirely** — I launched it detached and the reaper killed the whole process group, loop included, during a model load.

Then I asked the obvious question, and the whole story fell apart:

> **Me:** It should definetely be faster. How did we get it to work before?

> You're exactly right — it's **not the reaper, it's a CUDA OOM**. The traceback shows gpt-oss is running **eager attention**, which materializes the full N×N attention matrix → O(n²) memory. My ~16k ceiling estimate was wrong because it assumed memory-efficient attention.

> there was no special trick — earlier today the model genuinely loaded in ~21s because the box was less contended at that moment.

There was no reaper. There was a contended host that made model loads slow enough to get killed, and a quadratic attention blow-up that ran out of memory on long inputs — both perfectly ordinary once you looked. But the wrong belief wasn't useless. A random, unexplained failure is hard to act on; "something kills long jobs at random" is easy to act on — keep jobs short, checkpoint, retry. The false model produced the right behavior, and the right behavior shipped the probes.
