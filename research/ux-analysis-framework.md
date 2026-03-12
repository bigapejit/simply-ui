# UX Analysis Framework — SimplyUI

A 6-step research-backed framework for analyzing the simplicity and usability of any app or feature. Each step is grounded in published academic research. Can be applied post-build (audit what you have) or pre-build (analyze competitors / design intent).

---

## Step 1 — Define the Single Job

**Framework:** Jobs-to-be-Done (Christensen, Harvard Business School)

Christensen first published this in HBR in 2005 as "The Cause and the Cure of Marketing Malpractice," then formalized it in a 2016 HBR paper with Hall, Dillon & Duncan. The HBS module note is the most structured academic version.

**Sources:**
- HBR paper: https://www.relativimpact.com/downloads/Christensen-etal-Jobs.pdf
- HBS module note (most rigorous): https://ellisonchair.tamu.edu/files/2020/10/Integrating-Around-the-Job-to-Be-Done.pdf

**Exercise:** Before touching any UI, write the job in this format:

> "When [situation], I want to [motivation], so I can [outcome]."

One sentence. If it takes two sentences, the scope is wrong.

---

## Step 2 — Count Decisions (Interaction Cost)

**Framework:** GOMS / Keystroke-Level Model — Card, Moran & Newell (1983)

The foundational HCI textbook that introduced the idea of decomposing tasks into measurable cognitive and physical operations. Every decision, every keypress, every visual scan has a quantifiable cost.

**Sources:**
- Semantic Scholar: https://www.semanticscholar.org/paper/The-Psychology-of-Human-Computer-Interaction-Card-Moran/b2a61754c3f0c965f5de628e0d0e40f6f06789b1

**Exercise:** Map every screen in the primary flow. For each screen, write down every decision the user must make — not taps, decisions. Total them. Now cut any decision that isn't strictly required to complete the job.

---

## Step 3 — Audit What's Missing

**Framework:** Hick's Law (1952) + Iyengar & Lepper (2000)

The audit of missing features is the direct applied practice of Hick's Law — every feature you didn't add is a decision you didn't charge the user.

**Sources:**
- Hick (1952): https://www2.psychology.uiowa.edu/faculty/mordkoff/InfoProc/pdfs/Hick%201952.pdf
- Iyengar & Lepper (2000): https://faculty.washington.edu/jdb/345/345%20Articles/Iyengar%20&%20Lepper%20(2000).pdf

**Exercise:** List every feature a competitor has that your target app doesn't. For each one, write a one-sentence hypothesis for why it was cut. This forces you to decode the designer's model of the user.

---

## Step 4 — Follow the Error States

**Framework:** Nielsen's 10 Usability Heuristics — Nielsen & Molich (1990/1994)

Nielsen derived his heuristics from a factor analysis of 249 real usability problems. Heuristics 5 and 9 are specifically about error prevention and recovery — and how an app handles errors reveals whether simplicity was designed in or bolted on.

**Sources:**
- Original paper (Nielsen & Molich 1990): https://www.semanticscholar.org/paper/Heuristic-evaluation-of-user-interfaces-Nielsen-Molich/63ba7a959cde45e9218f5c78e2be8b37d30c9dcc
- Full heuristic set formalized (1994): https://www.semanticscholar.org/paper/Ten-Usability-Heuristics-Nielsen/6c49b0081d2887d0e5e22c84ac2f7c992b943599

**Exercise:** Deliberately trigger every error state. For each one, score it on two dimensions:
1. How many words does the error message use?
2. Does it tell you what to do next?

Simple apps have short, action-oriented errors. Complex ones have long explanations that still leave you stuck.

---

## Step 5 — Categorize Every Screen

**Framework:** Cognitive Load Theory — Sweller (1988)

Sweller proved working memory is finite and that every unnecessary element actively consumes it. Categorizing screens into Orient / Decide / Act / Confirm exposes which screens are spending cognitive budget without advancing the user toward completing the job.

**Sources:**
- Semantic Scholar: https://www.semanticscholar.org/paper/Cognitive-Load-During-Problem-Solving:-Effects-on-Sweller/d88c481743db95687bf9d2861c16cd006f67a0a1

**Exercise:** Screenshot every screen, print them out, and physically tag each one:
- **Orient** — helps user understand where they are
- **Decide** — presents choices
- **Act** — user performs the task
- **Confirm** — verifies completion

Any screen tagged "Orient" that isn't the very first onboarding screen is a red flag — it means the designer wasn't confident the user knew where they were.

---

## Step 6 — Decode the Defaults

**Framework:** Johnson, Bellman & Lohse (2002) — "Defaults, Framing and Privacy"

This Columbia/Wharton paper proved empirically that defaults don't just reflect designer preferences — they actively construct user preferences. Every default is a bet on what the average user wants most of the time.

**Sources:**
- SSRN (open access): https://papers.ssrn.com/sol3/papers.cfm?abstract_id=1324778
- ResearchGate PDF: https://www.researchgate.net/publication/227079453_Defaults_Framing_and_Privacy_Why_Opting_In-Opting_Out1

**Exercise:** List every default in the app — sort order, notification settings, display modes, pre-filled fields, everything. For each one, write down who that default advantages. That list is a map of the designer's assumptions about their user, which tells you more about the product's philosophy than any design document would.

---

## Usage Modes

### Post-Build (audit what you already have)
Analyze your own app's screens, flows, error states, and defaults against the framework. Claude reads the codebase to extract screens, decisions, defaults, and error handling.

### Pre-Build (competitive analysis / design intent)
Analyze a competitor app or reference design before building. Requires screenshots, URLs, or descriptions as input. Claude helps you decode the design philosophy.
