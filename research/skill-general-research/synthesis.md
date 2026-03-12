# Synthesis: How to Improve SimplyUI

Based on research from Made to Stick (Heath & Heath) and The E-Myth Revisited (Gerber).

## The Core Problem

The skill's job: extract what's in a developer's brain, find the disconnects between what they think the user needs and what the user actually needs, and produce a document any builder can execute from.

Current approach: open-ended questions that depend on the developer being articulate about design.

Why it fails:
- **Curse of Knowledge** — developers can't unsee how they built it
- **Abstract questions get abstract answers** — no sensory hooks, nothing sticks
- **Too much improvisation** — the skill tells Claude to "ask these kinds of questions" instead of specifying exact questions
- **Too many files** — six per-task files eat context window for process artifacts that don't matter after the brief is written
- **Map updates ask the wrong questions** — "is this still accurate?" puts the burden on the developer instead of the system

## Three Fixes

### Fix 1: Flip the Questioning Model (Made to Stick: Concrete + Curse of Knowledge)

**Instead of asking the developer to describe, the skill should describe and ask the developer to correct.**

Current: "What's on this screen?" → developer describes from cursed knowledge
Better: "I read your code. Here's what I think the user sees: [concrete description]. What did I get wrong?"

Correcting is easier than describing. And it surfaces gaps — the things so obvious to the developer they're invisible.

For map updates specifically:
- Current: git diff → "is this still accurate?"
- Better: read changed files → generate new description → diff against old map → "Here's what I think changed: [specific thing]. Right or wrong?"

### Fix 2: Commander's Intent First (Made to Stick: Simple + E-Myth: Orchestration)

**One question before everything: "If this feature does only ONE thing perfectly, what is it?"**

This is the Commander's Intent. It comes FIRST, not after brainstorming. Everything in diverge tests against it. Everything in converge cuts against it.

Current flow: diverge → converge → JTBD → decisions → brief
Better flow: intent → gaps → cut → brief

The JTBD formalizes what they already said in the intent. It doesn't need its own phase.

### Fix 3: Fixed Extraction System (E-Myth: Franchise Prototype + Orchestration)

**Replace improvised question-asking with a fixed set of gap-finding questions.**

The franchise prototype works because it's the same every time. The skill should have EXACT questions, not "ask these kinds of questions."

Gap-finding questions (open knowledge gaps the developer didn't know they had):
1. "What does the user do RIGHT BEFORE they need this?"
2. "What do they do RIGHT AFTER?"
3. "What happens when this goes wrong?"
4. "What happens if they change their mind halfway through?"
5. "What's the thing about this that would confuse someone seeing it for the first time?"

These are concrete (force specific answers), unexpected (open gaps), and systematic (same every time).

## File Structure Change

### Kill intermediate files

Current per-task: status.md, diverge.md, converge.md, job.md, decisions.md, brief.md (6 files)
Better per-task: brief.md (1 file)

The process happens in conversation. Only the output persists.

### Total file structure

```
.simply/
  project.md          ← the anchor (keep)
  map/
    INDEX.md          ← scannable in 10 seconds (simplify)
    screens/*.md      ← what the user sees (keep)
    flows/*.md        ← how things connect (keep)
  work/
    <slug>/
      brief.md        ← the output (keep, only file)
```

## The Improved Process (One View)

1. **Intent** (1 question) — "What's the one thing this does perfectly?"
2. **Context** (skill reads code/map, presents what it sees, developer corrects)
3. **Gaps** (fixed gap-finding questions — same every time, like a franchise)
4. **Cut** (converge against the Commander's Intent)
5. **Brief** (same output format — already good)

Every question is:
- **Concrete** (forces a specific answer, not a brainstorm dump)
- **Unexpected** (opens a knowledge gap the developer didn't realize they had)
- **Systematic** (same questions every time, like a franchise)

## Key Principles Applied

| Principle | Source | How It Applies |
|-----------|--------|----------------|
| Commander's Intent | Made to Stick | One sentence goal before anything else |
| Curse of Knowledge | Made to Stick | Developer can't describe their own app objectively — flip to correction |
| Velcro Theory | Made to Stick | Concrete, sensory questions create more memory hooks |
| Knowledge Gap Theory | Made to Stick | Questions should open gaps, not ask for dumps |
| Franchise Prototype | E-Myth | Same process every time, regardless of the problem |
| Lowest Skill Level | E-Myth | Questions should be answerable without design expertise |
| Orchestration | E-Myth | Bake in what works, eliminate improvisation |
| Systems over People | E-Myth | The system produces the result, not the developer's articulation |
| Creativity vs. System | E-Myth | System handles the predictable; creativity goes to the answers |
