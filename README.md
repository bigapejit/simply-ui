# SimplyUI

You don't have a building problem. You have a thinking problem.

You tell Claude "I need a settings page," it builds one, and you spend the next hour going "no, not like that." Then you try again. More rewrites. More steering. The AI is fast at building the wrong thing, and you're burning all that speed on course corrections.

The issue isn't the AI. It's that you didn't actually know what you wanted. You had a vibe, not a spec. And vibes don't survive contact with code.

SimplyUI is a planning tool for Claude Code. Two slash commands that force you to think through what you're building before you build it. A structured conversation that ends with a clear, one-page brief you can hand to any AI or human and say "build this."

## How it works

You run `/simply-ui` and it walks you through a series of questions. Not open-ended "tell me about your feature" questions. Specific, pointed ones designed to surface the stuff you haven't thought about yet.

The process:

1. **"If this feature does only ONE thing perfectly, what is it?"** This is your Commander's Intent. One sentence. If you say "and," you've got two features. Pick one.

2. **Gap-finding.** Five fixed questions, same every time: What does the user do right before this? Right after? What happens when it breaks? When they change their mind halfway through? What would confuse a first-timer? Then an open brainstorm to get everything else out.

3. **Cut.** Now you go through every idea and ask "what happens if we just don't build this?" If the answer is "the user figures it out," it's gone. You keep only what the user can't complete the core job without.

4. **The brief.** Everything that survived gets written into a single document: the job, what to build, what NOT to build, defaults, screen breakdown, error states. Saved to `.simply/work/<slug>/brief.md`. That's the only file the framework writes per task. All the thinking stays in the conversation.

The brief is portable. Copy it into a new Claude session and say "build this." Hand it to a teammate. Feed it into VBW. Whatever. SimplyUI doesn't care what happens after. It just makes sure you know what you're building first.

### The second command: `/simply-ui-map`

This one documents your app from the user's perspective. Not your perspective as the developer who built it. The user's.

You walk Claude through what someone sees and does in your app, screen by screen. It creates a lightweight index of flows, screens, and decision counts. When you later run `/simply-ui` to plan something new, the framework reads the map to understand what already exists around the thing you're planning.

There's a trick to how updates work. Instead of asking you "is the map still accurate?" (which puts all the work on you), the framework reads your code changes, generates what it *thinks* changed, and asks you to correct it. Correcting is way easier than describing from scratch. This is deliberate. Developers can't objectively describe their own app because they built it and can't unsee how it works. Flipping to correction mode defeats that.

## Why it's built this way

I kept overcomplicating my own UI. I'd plan a feature in my head, start building, realize I hadn't thought something through, bolt on more stuff, and end up with screens that tried to do too much. The AI made this worse because it could build fast enough to outrun my thinking.

So I built a process that forces the thinking to happen first, and made it push back when I overcomplicate things. The personality is direct and skeptical on purpose. It asks "is that necessary?" more than "what else could we add?"

The framework pulls from a few sources that shaped how it works:

- **Commander's Intent** (Made to Stick, Heath & Heath) is why there's a single sentence goal before anything else. The military uses it because plans fall apart on contact with reality. Same thing happens with feature specs.
- **Curse of Knowledge** (same book) is why the map command flips to correction mode instead of asking you to describe things. Once you know how something works, you literally cannot imagine what it's like not to know. You need someone else to describe it wrong so you can fix it.
- **Jobs-to-be-Done** (Christensen, HBS) is why every feature gets reduced to one job in one sentence. If you can't state it that simply, the scope is wrong.
- **Hick's Law** is why the framework counts decisions per screen and flags anything over 7. Every option you add is cognitive tax on the user.
- **The Franchise Prototype** (The E-Myth, Gerber) is why the questions are the same every time. The system produces the result, not your ability to articulate design ideas on the fly.

The `research/` folder has the full breakdown if you want to dig in.

## Using SimplyUI with VBW

If you use [VBW (Vibe Better With Claude Code)](https://github.com/yidakee/vibe-better-with-claude-code-vbw), SimplyUI fills the gap before VBW's lifecycle starts.

VBW handles everything from scoping through verification. But it assumes you already know what you're building. SimplyUI is where you figure that out.

1. Run `/simply-ui` to think through the feature. Get a brief.
2. Run `/vbw:vibe` with the brief as input. VBW's architect scopes it, the lead plans it, devs build it, QA verifies it.

The brief gives the architect what it needs without ambiguity: a job statement, what to build, what not to build, screen breakdown, constraints. No scope drift during the build.

You can also run `/simply-ui-map` before a VBW milestone to give the architect full context on what users currently see in the app.

## Installation

Two markdown files. No packages, no dependencies.

1. Clone or download this repo:
   ```bash
   git clone https://github.com/bigapejit/simply-ui.git
   ```

2. Copy the skill folders into your project:
   ```bash
   mkdir -p your-project/.claude/skills
   cp -r simply-ui/skills/simply-ui your-project/.claude/skills/
   cp -r simply-ui/skills/simply-ui-map your-project/.claude/skills/
   ```

3. Use them in Claude Code:
   ```
   /simply-ui I need a settings page
   /simply-ui-map
   ```

The first time you run `/simply-ui`, it asks a few questions about your app (what it does, who uses it, what the core actions are) and saves a `project.md` file. After that, it reads that file at the start of every session so it knows the context.

### What gets created in your project

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

### How the pieces connect

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

This was built for mobile app UIs. The principles apply to other stuff, but the specific advice (thumb zones, tap depth limits) assumes a phone screen.

It only plans. It won't write code or suggest how to implement anything. If you want a tool that also builds, this isn't it.

It's a single-dev tool. No multi-user collaboration, no Figma integration, no design handoff process.

## License

MIT
