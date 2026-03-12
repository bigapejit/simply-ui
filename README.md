# SimplyUI

**This is not a framework and it's not trying to replace one.**

SimplyUI is just structured conversation. The back-and-forth you'd already have with Claude before building something — "what should this settings page do? who's it for? what can we skip?" — except now that conversation follows a path and produces a document at the end instead of disappearing into your chat history.

You still use whatever build system you want after. VBW, GSD, raw Claude, your own thing. SimplyUI just gets your thinking organized before that step. It produces documentation, not architecture.

If you use something like Wispr Flow to talk your prompts, you're going to get a lot out of this. SimplyUI basically lets you yap about your feature idea in a way that's structured enough for Claude to actually do something with later. Talk through what you're building, answer the questions it asks, and it organizes all of that rambling into a brief. If voice-first prompting already clicks for you, this is going to feel very natural.

## Get started

Two markdown files you drop into your project. Copy them in and they explain themselves.

1. Grab the repo:
   ```bash
   git clone https://github.com/bigapejit/simply-ui.git
   ```

2. Copy the skills into your project:
   ```bash
   mkdir -p your-project/.claude/skills
   cp -r simply-ui/skills/simply-ui your-project/.claude/skills/
   cp -r simply-ui/skills/simply-ui-map your-project/.claude/skills/
   ```

3. Run them:
   ```
   /simply-ui I need a settings page
   /simply-ui-map
   ```

That's it. The skills walk you through everything. First time it asks about your app, after that it remembers. You don't need to read anything below this to use it. But if you want to know more about how it works and why, keep going.

---

## What it actually does

You know that thing where you tell Claude "I need a settings page" and it just builds one? And then you go "no, not like that" and spend an hour steering? The problem isn't Claude. It's that you didn't actually know what you wanted yet. You had a vibe, not a spec.

SimplyUI is the conversation you should have had first. It asks you questions that surface the stuff you haven't thought about, then organizes your answers into a one-page brief that any AI or person can build from.

The questions follow a fixed path every time:

1. "If this feature does only ONE thing perfectly, what is it?" One sentence. If you say "and," that's two features. Pick one.

2. Five gap-finding questions: What does the user do right before this? Right after? What happens when it breaks? When they change their mind? What would confuse someone seeing it for the first time? Then an open brainstorm to dump everything else out.

3. Cut. Go through every idea and ask "what happens if we don't build this?" If the user figures it out on their own, it's gone.

4. The brief. What survived gets written to `.simply/work/<slug>/brief.md`. The job, what to build, what not to build, defaults, screen layout, error states. One file. All the conversation stays in the conversation.

Take that brief wherever you want. New Claude session, VBW, GSD, a teammate, a napkin. The brief is the handoff point.

### `/simply-ui-map` — the other command

This one documents your existing app from the user's perspective. Not how you built it. What someone sees when they open it.

You walk through screens, flows, decisions. It creates a lightweight map. When you later plan something new with `/simply-ui`, the framework reads that map to understand what's already there. This matters most when you're changing existing UI, not building from scratch.

Updates are clever: instead of asking "is this still right?" the framework reads your recent code changes, describes what it thinks changed, and asks you to correct it. Way easier than describing from scratch. This is intentional — you literally cannot objectively describe your own app because you know too much about how it works under the hood.

## Pairs with any build workflow

SimplyUI sits before your build process, not inside it.

**With [VBW](https://github.com/yidakee/vibe-better-with-claude-code-vbw):** Run `/simply-ui` first, get your brief, then `/vbw:vibe` with the brief as input. VBW's architect gets a clear spec with no ambiguity. No scope drift during execution.

**With GSD:** Same idea. `/simply-ui` produces the brief, then feed it into `/gsd:new-project` or `/gsd:plan-phase`. The brief gives the planner what it needs without you having to re-explain everything.

**With nothing:** Just copy the brief into a new Claude session and say "build this." It works fine standalone.

## Why I built this

I kept overcomplicating my own UI. I'd plan a feature in my head, start building, realize I hadn't thought something through, bolt on more stuff, and end up with screens doing too much. AI made this worse — it builds fast enough to outrun your thinking. You can have a bad idea fully implemented before you realize it was bad.

So I built something that forces the thinking to happen first. The personality is direct and skeptical on purpose. It asks "is that necessary?" way more than "what else could we add?"

It's the kind of conversation I needed to have with someone who'd push back on me. Except I don't have a design partner, so I made Claude do it.

## The thinking behind it

If you're curious why the process works the way it does:

- **Commander's Intent** (from Made to Stick) — the military figured out that plans fall apart on contact with reality, so they lead every order with a single sentence describing the desired outcome. SimplyUI does the same: one sentence goal before anything else, and every idea gets tested against it.
- **Curse of Knowledge** (same book) — once you know how something works, you can't imagine what it's like not to know. That's why the map command doesn't ask you to describe your app. It describes it wrong and lets you correct it. Way more effective.
- **Jobs-to-be-Done** (Christensen, Harvard) — every feature serves one job. If you need two sentences to describe the job, you've got two features. The framework enforces this.
- **Hick's Law** — every option you put on a screen is cognitive tax. The framework counts decisions per screen and flags anything over 7.
- **Franchise Prototype** (from The E-Myth) — the questions are the same every time because the system should produce the result, not your ability to think about design on the fly.

The `research/` folder has the full deep-dive on each of these.

## What gets created in your project

```
your-project/
  .claude/skills/
    simply-ui/SKILL.md         ← the planning command
    simply-ui-map/SKILL.md     ← the mapping command
  .simply/                     ← created on first use
    project.md                 ← app identity (created once)
    backlog.md                 ← parked ideas
    map/                       ← UI map (optional)
      INDEX.md
      flows/*.md
      screens/*.md
    work/
      <slug>/brief.md          ← one brief per task
```

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

## Limitations

Built for mobile app UIs. The principles work elsewhere but the specific advice (thumb zones, tap depth) assumes a phone.

Only plans. Won't write code or suggest implementations. If you want a tool that also builds, this isn't it.

Single-dev tool. No multi-user collaboration or design handoff.

## License

MIT
