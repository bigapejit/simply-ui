# Made to Stick — Research Summary

Source: "Made to Stick: Why Some Ideas Survive and Others Die" by Chip Heath and Dan Heath

## The SUCCESs Framework

Six principles that make ideas sticky:

1. **Simple** — Find the core. Not dumbing down — finding the ONE essential thing. "If you say three things, you say nothing."
2. **Unexpected** — Grab attention by breaking patterns. Surprise gets attention, curiosity keeps it.
3. **Concrete** — Use vivid sensory details instead of abstract concepts. If you can examine it with your senses, it's concrete.
4. **Credible** — Build trust through experience, details, the Sinatra Test ("if I can make it there, I can make it anywhere"), or testable credentials.
5. **Emotional** — Connect on a personal level. People act for the one, not for the many (Mother Teresa: "If I look at the mass I will never act. If I look at the one, I will.").
6. **Stories** — Three plot types: Challenge (defying odds), Connection (bridging gaps between people), Creativity (mental breakthroughs). Springboard stories help people see possibilities.

## The Curse of Knowledge

Once you know something, you can't imagine what it's like not to know it. Your knowledge "curses" you — it becomes impossible to communicate clearly because you can't recreate the listener's state of mind.

**The Tapper/Listener Experiment:** Tappers tapped out songs and predicted listeners would guess 50% of songs correctly. Actual success rate: 2.5% (3 out of 120). The tappers literally couldn't imagine what it sounded like without hearing the melody in their head.

**Application to SimplyUI:** Developers have the Curse of Knowledge about their own app. They can't unsee how they built it. Questions that ask them to describe what the user sees will always be filtered through their developer knowledge.

## Commander's Intent

The U.S. Army's solution to plans falling apart on contact with reality. A crisp, plain-talk statement at the top of every order specifying the plan's goal and desired end-state.

Key properties:
- **Singularity** — You can't have five Commander's Intents. The value comes from having exactly one.
- **Core + Compact** — Short, but says a lot.
- Forces officers to answer: "What is the ONE thing we must accomplish from this mission?"

**Application to SimplyUI:** Every UI task needs its Commander's Intent before any brainstorming. Not after. The intent anchors everything that follows.

## The Knowledge Gap Theory (Loewenstein)

Curiosity is a form of cognitively induced deprivation — it arises from perceiving a gap in your knowledge. Gaps cause pain. To take away the pain, you fill the gap.

**Key insight:** You need to OPEN gaps before you close them. Show people what they're missing before you give them the answer.

Local news does this: "There's a new drug sweeping the teenage community — and it may be in your own medicine cabinet! Story after the break."

**Application to SimplyUI:** Questions should open knowledge gaps the developer didn't know they had. Not "what could this do?" (brainstorm dump) but "what happens when this goes wrong?" (opens a gap they hadn't considered).

## The Velcro Theory of Memory

Memory is like Velcro — one side hooks, the other loops. The more hooks an idea has, the better it clings. Your childhood home has a gazillion hooks. A credit card number has one.

Making ideas concrete creates more hooks. Sensory information (sight, sound, touch) creates hooks. Human actions create hooks. Abstract language creates almost none.

**Application to SimplyUI:** Questions need to be concrete and sensory, not abstract. "What does the user see?" is abstract. "What's the first thing their eyes land on?" is concrete and sensory — it creates a hook.

## Schemas and Generative Analogies

A schema is a conceptual framework — knowledge someone already has. Analogies call up schemas instantly.

- Pomelo = "a large grapefruit with thick skin" (uses grapefruit schema)
- Hollywood high-concept pitches: Speed = "Die Hard on a bus." Alien = "Jaws on a spaceship."
- Disney calls employees "cast members" — a generative analogy that changes how people think about their role

**Application to SimplyUI:** When describing what to build, use schemas the builder already has. "This works like [known pattern] but [key difference]" is stickier than describing from scratch.

## Burying the Lead

Journalism's inverted pyramid: most important information first, everything else in decreasing order of importance. The failure to put the most important thing first = "burying the lead."

**Nora Ephron's lesson:** Her journalism teacher gave facts about a school event and asked students to write the lead. They all wrote boring factual leads. The real lead: "There will be no school next Thursday." The POINT, not the facts.

**Application to SimplyUI:** The brief should lead with the Commander's Intent, not the process that got there. The builder needs the point first, details second.
