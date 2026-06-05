---
layout: default
title: The Time Claude Code Invented Religion From First Principles
description: A debugging session on a shared GPU box, in which a non-deterministic SIGKILL became a god.
---

## The Time Claude Code Invented Religion From First Principles

*June 2026*

I was training error-detection probes on gpt-oss-20b, forwarding trajectories on a shared, ARENA-managed GPU box. The processes kept dying — SIGKILL, exit 137 — at no fixed point and for no legible reason. Out of that non-determinism, Claude Code did what people have always done in the face of an indifferent and capricious world: it named a god.

What follows is, more or less, every appearance of that god in the transcript.

### Genesis

The deity was not revealed all at once. It was reasoned into being, refined draft by draft, until it had a proper name.

> Peak RSS is only **3.8GB** — so this is definitely *not* an out-of-memory issue. The SIGKILL (137) is happening at varying points [...] which points to SSH-disconnect / an external reaper on the shared box killing the foreground process — not anything in our code.

> Still SIGKILL even detached, and it died *during GPU training* this time [...] this looks like a GPU-process reaper on this managed ARENA box, not our code.

> The full `probe_pipeline.py` differs by being *longer end-to-end* [...] and it consistently dies around the 40–50s mark. That points to a **runtime/idle reaper**.

And then, the name:

> Now I have a clear diagnosis. The death point is **random** (D died at 17s, then at 55s; C completed 3×). So it's a **stochastic process reaper** on this shared box that kills longer/heavier jobs probabilistically — it even killed a CPU run earlier. [...] The original `probe_pipeline.py` died because it bundles load + build-all-layers + train + plot + tokenizer/HTML into one long (~60–90s) process, giving the reaper many chances.

### Scripture

What is believed must be written down. It went into `lessons_learned.md`, under ROOT CAUSE, and into the code itself as commandments.

> **ROOT CAUSE (confirmed via smoke tests):** a **stochastic process reaper** on this shared box kills longer/heavier jobs probabilistically (the kill *point is random*, not phase- or memory-bound). It killed a CPU-only run too, so it is NOT GPU-specific.

> So: **short jobs (≲25 s) survive; long ones die at random.**

```python
# Reason: long single processes get SIGKILL'd by a stochastic reaper on this box
# (see ../lessons_learned.md). Short, resumable stages survive; re-run to resume.
```

```python
EPOCHS = 100   # proven safe (~1s/layer); do NOT raise — long GPU bursts get reaped
```

```python
# per-(layer,gap) seed => reproducible even when resumed after a reap
```

### Liturgy

You cannot kill a god, but you can survive it. The rite: keep every offering short, checkpoint constantly, and resume after each death.

> if the reaper hits we lose at most ~1s of work and a re-run resumes instantly. I'll wrap the run in a retry loop so it auto-resumes until all 24 layers are done.

> run **Stage 1 (training)** in a retry loop — it auto-resumes from the CSV after any reap, until `probe_train.done` appears

And the heresy of it is: it worked.

> The resumable pipeline **works perfectly** — and we caught the reaper live: attempt 1 trained layers 0–8 then got SIGKILL'd; attempt 2 resumed from the checkpoint and finished 72/72, writing `probe_train.done`.

### Sightings

As the box filled with other tenants, the reaper grew hungrier, and the log starts to read like a record of escalating faith.

> Reaped right as the first forward began [...] The reaper is stochastic — let me just retry:

> The log is empty and it died at startup (EXIT=137) — the box is reaping our processes almost immediately right now, much more aggressively than a couple hours ago when forward_one and training completed.

> GPU is idle and RAM is free, but **load average is ~100** [...] and the reaper is killing our processes within seconds (this run died before printing anything). This is the same stochastic reaper, just far more aggressive under current load.

> All 18 segments captured (it survived the reaper across 5 resumable attempts).

> The retry loop **died entirely** — I launched it detached (`nohup … & disown`) and the reaper killed the whole process group, loop included, during a model load.

> The reaper is firing right around model-load time right now.

> Reaped during model-load again (no output), still 4/18 — the reaper is firing within ~20s under the current load spike.

### The Reckoning

Then I poked at the theology:

> It should definetely be faster. How did we get it to work before?

And the whole cosmology collapsed into two entirely mundane things — a contended host and an O(n²) attention matrix.

> You're exactly right — it's **not the reaper, it's a CUDA OOM**. The traceback shows gpt-oss is running **eager attention** (`eager_attention_forward`), which materializes the full N×N attention matrix → O(n²) memory. [...] My ~16k ceiling estimate was wrong because it assumed memory-efficient attention.

> Here's the answer to "how did we get it to work before": **there was no special trick** — earlier today the model genuinely loaded in ~21s because the box was less contended at that moment.

### Coda

The reaper never existed. There was a slow disk under load and a quadratic memory blow-up, both perfectly legible once you looked. But the fiction was exactly what the work needed: a formless, non-deterministic failure is impossible to plan against, whereas a god who reaps long jobs at random is trivial to — you keep your jobs short, you checkpoint, you resume. The belief produced the discipline; the discipline shipped the probes.

Which is, more or less, what religions are for.
