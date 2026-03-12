# SimplyUI

A UI planning framework for Claude Code. It helps you think clearly about what to build **before** you build it.

SimplyUI is not a component library, not a design system, not a code generator. It's a thinking tool — a structured conversation that turns a vague idea ("I need a settings page") into a clear, buildable brief that any developer or AI can execute from.

## Why This Exists

If you've used AI to build UI, you've hit this: you describe what you want, the AI builds *something*, and then you spend hours steering it toward what you actually meant. The problem isn't the building — it's that you didn't know exactly what you wanted yet. You had a feeling, not a spec.

Developers — especially solo devs and vibe coders — tend to skip straight from "I need this thing" to writing code. The thinking happens *during* the build, which means you're paying for it in rewrites, scope creep, and features nobody asked for.

SimplyUI forces the thinking to happen first. It asks the questions you'd skip, challenges the assumptions you didn't know you were making, and produces a document that captures *what* to build and *why* — so the build phase (whether you use Claude, Cursor, your own hands, whatever) goes faster and cleaner.

The framework is built on actual research — Jobs-to-be-Done theory, cognitive load research, Hick's Law, the "Commander's Intent" concept from Made to Stick, and the franchise prototype model from The E-Myth. The `research/` folder has all the details if you're curious about the design decisions.

## What It Does

Two slash commands:

### `/simply-ui` — Plan a UI task

A guided conversation that takes you through:

1. **Commander's Intent** — "If this feature does only ONE thing perfectly, what is it?" One sentence. No "and."
2. **Diverge** — Five structured gap-finding questions (what happens before, after, when it breaks, when they change their mind, what confuses a first-time user), then an open brainstorm.
3. **Converge** — Cut ruthlessly against the intent. Core, supporting, or cut. If the answer to "what happens if we don't build it?" is "they figure it out" — cut it.
4. **JTBD** — Formalize the job: "When [situation], I want to [motivation], so I can [outcome]." One sentence.
5. **Decision Map** — Map every decision the user faces. Flag overloaded screens (>7 decisions), excessive depth (>3 taps for frequent actions), and bad placement.
6. **Brief** — A clean document saved to `.simply/work/<slug>/brief.md`. Contains everything needed to build — the job, what to build, what NOT to build, defaults, screen breakdown, error states, consistency notes.

The brief is the output. Take it to a new Claude session, paste it in, say "build this." Or hand it to a teammate. Or use it as a spec. The framework doesn't care what happens after — it just makes sure you know what you're building before you start.

### `/simply-ui-map` — Map your app's UI

Documents your app from the **user's perspective** — not the developer's. Creates a lightweight index of every screen, flow, and decision point a user encounters.

- **First run**: Walks you through the app screen by screen. "Pretend I just downloaded this. What do I see?"
- **Updates**: Reads your code changes, generates what it *thinks* changed, and asks you to correct. Correcting is easier than describing (this is deliberate — it defeats the Curse of Knowledge).
- **Targeted mapping**: `/simply-ui-map the settings area` maps just one area.

The map feeds into `/simply-ui` — when you plan a new feature, the framework reads the map to understand what already exists around it. This is especially important when simplifying existing UI.

### How The Pieces Fit Together

```
                    First time only
                    ┌─────────────┐
                    │  /simply-ui  │──── creates ────▶ .simply/project.md
                    │  (asks about │                   (app identity: what it does,
                    │   your app)  │                    who uses it, core actions)
                    └─────────────┘

     ┌──────────────────┐          ┌──────────────────┐
     │  /simply-ui-map   │◀── reads ──│   /simply-ui      │
     │                    │          │                    │
     │  Maps what users   │          │  Plans what to     │
     │  see and do today  │          │  build next        │
     │                    │          │                    │
     │  Output:           │          │  Output:           │
     │  .simply/map/      │          │  .simply/work/     │
     │    INDEX.md        │          │    <slug>/brief.md │
     │    flows/*.md      │          │                    │
     │    screens/*.md    │          │  Also manages:     │
     └──────────────────┘          │  .simply/backlog.md │
                                    └──────────────────┘
```

- `project.md` is created once and read every time — it's the anchor
- The **map** is optional but makes planning better (the framework nudges you to create it)
- The **brief** is the only output per task — all thinking stays in the conversation
- The **backlog** catches good ideas that don't belong in the current task

## Using SimplyUI with VBW

If you use [VBW (Vibe Better With Claude Code)](https://github.com/yidakee/vibe-better-with-claude-code-vbw), SimplyUI fills the gap *before* VBW's lifecycle kicks in.

VBW handles the build: scope → plan → execute → verify → archive. But it assumes you know what you're building. SimplyUI is where you figure that out.

**The workflow:**

1. **SimplyUI** — `/simply-ui` to think through the feature. Get a brief.
2. **VBW** — `/vbw:vibe` with the brief as input. VBW's architect scopes it, the lead plans it, devs build it, QA verifies it.

The brief gives VBW's architect exactly what it needs: a clear job statement, what to build, what NOT to build, screen breakdown, and constraints. No ambiguity, no scope drift during the build phase.

You can also use `/simply-ui-map` before starting a VBW milestone to give the architect full context on what the app currently looks like from the user's perspective.

## Installation

SimplyUI is a set of Claude Code skills (markdown files). No packages, no dependencies.

### Quick setup

1. **Download** this repo (clone, zip, whatever):
   ```bash
   git clone https://github.com/bigapejit/simply-ui.git
   ```

2. **Copy the skills** into your project's `.claude/skills/` directory:
   ```bash
   mkdir -p your-project/.claude/skills
   cp -r simply-ui/skills/simply-ui your-project/.claude/skills/
   cp -r simply-ui/skills/simply-ui-map your-project/.claude/skills/
   ```

3. **Use them** in any Claude Code session inside that project:
   ```
   /simply-ui I need a settings page
   /simply-ui-map
   ```

That's it. The skills will create a `.simply/` directory in your project for all their working files.

### What gets created in your project

```
your-project/
  .claude/skills/
    simply-ui/SKILL.md         ← the planning command
    simply-ui-map/SKILL.md     ← the mapping command
  .simply/                     ← created on first use
    project.md                 ← app identity (created once)
    backlog.md                 ← parked ideas
    map/                       ← UI map (if you use /simply-ui-map)
      INDEX.md
      flows/*.md
      screens/*.md
    work/                      ← planning workspaces
      <slug>/brief.md          ← one brief per task
```

## The Research Behind It

The `research/` folder contains the academic and business frameworks that shaped the design:

- **Jobs-to-be-Done** (Christensen, HBS) — Every feature exists to serve one job. If you can't state the job in one sentence, the scope is wrong.
- **Hick's Law** — Every option you add is a decision you charge the user. Features you *don't* build are decisions you *don't* charge.
- **Cognitive Load Theory** (Sweller) — Working memory is finite. Every unnecessary element on a screen actively consumes it.
- **Nielsen's Heuristics** — How an app handles errors reveals whether simplicity was designed in or bolted on.
- **Commander's Intent** (from Made to Stick, Heath & Heath) — One sentence goal before anything else. Everything gets tested against it.
- **Curse of Knowledge** (Made to Stick) — Developers can't objectively see their own app. The framework counteracts this by having Claude describe and the developer *correct*, rather than asking developers to describe from scratch.
- **Franchise Prototype** (The E-Myth, Gerber) — Same process every time. The system produces the result, not the developer's ability to articulate design ideas.

## Design Principles

These are baked into the framework, not just aspirations:

- **Diverge first, then converge.** Get everything out before cutting anything. People skip the brainstorm and go straight to building — this forces the exploration.
- **Frequency drives everything.** "How often does the user do this?" changes every design decision. Every-session actions get zero friction. Rarely-used features can be buried.
- **No overloaded screens.** More than 7 decisions on one screen = split it into guided steps.
- **No unnecessary depth.** More than 3 taps to reach a frequent action = too deep.
- **Originality is a tool, not a goal.** The conventional approach is usually right. Only explore unconventional solutions when the conventional one is clearly failing.
- **The brief is the output.** All thinking stays in the conversation. Only the final document persists.
- **Scope guardian.** When ideas drift, they get parked in the backlog — not lost, not built, just parked.

## Limitations

- **Mobile React focus.** The framework was designed for mobile app UIs. The principles apply broadly, but the specific guidance (thumb zones, tap depth, etc.) assumes a mobile context.
- **Planning only.** This does not write code, generate components, or suggest implementations. That's by design — but if you want a tool that also builds, this isn't it.
- **Single developer workflow.** This was built for solo devs and small teams. There's no multi-user collaboration, no Figma integration, no design handoff.

## License

MIT
