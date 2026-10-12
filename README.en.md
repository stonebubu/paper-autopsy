# 🧟 Paper Autopsy

<p align="center"><b>Don't read papers. Autopsy them.</b></p>

<p align="center"><a href="README.md">中文</a> | English</p>

Most papers are weak. The strongest minority still carry packaging. In the age of AI, packaging has become a low-cost, learn-it-or-die research skill — a good story, polished figures, and trendy keywords often persuade harder than the actual result.

And seeing through packaging is tuition every researcher pays, in a course no one has ever formally taught. Newcomers take abstracts and conclusions at face value, get carried by polished figures, mistake redefined problems for field-level difficulties, and spend years in quiet self-doubt — *"why does everyone cite this? I can't see what's so good about it"* — before the instinct to ask *"where's the evidence?"* finally grows in. The tuition is measured in years, and at the end you get no lecture notes.

Paper Autopsy is an attempt to write those lecture notes: to turn the judgment of an experienced reader into an agent skill anyone can install on day one. Veterans use it to save time on routine dissections. Newcomers arguably benefit more — every verdict report is a demonstration of *how a suspicious reader actually reads*, and if you read enough of them, that voice becomes your own.

Paper Autopsy is a [Claude Code](https://claude.com/claude-code) / [agent-skills](https://github.com/anthropics/skills) skill. In one sentence: it makes your agent read a paper with suspicion as the default stance, and hand you a graded autopsy report — what the paper is after the packaging comes off, not what it wants you to believe.

**Not an interpreter. A prosecutor.** Interpreters assume the paper deserves to be understood; the prosecutor demands evidence — same paper, two questions: *"what does it say?" or "what does it actually show — and what does it merely claim?"*

## What it does

Given a paper (PDF path, arXiv link, or title), the agent:

1. **Reads the full text** — body, figures, appendix. The abstract is used only to record the author's self-claims, never as a basis for judgment;
2. **Analyzes the two motives**: what the field genuinely aches from (necessity), and how the solution was actually conceived (possibility) — see the dual-motive framework below;
3. **Grades three dimensions** (Territory / Mind / Evidence, each S–D), then compresses them into one sentence of net contribution and an action recommendation (read closely / cite / skip);
4. **Writes the verdict report**: every judgment anchored to a specific location in the paper; judgments that cannot be anchored get downgraded to "suspected."

No configuration: copy it into your skills directory and talk to your agent normally.

## The dual-motive framework

Contribution tells you what a paper did. **Motive tells you what's real.** Autopsy centers on two motives:

**1. The motive of necessity — the researcher's territory 🌍**
The genuine pain point of a field. Is it a long-acknowledged difficulty that other works independently grapple with — or a narrative manufactured to fit the method? A field's real bottleneck, or a technical gap of "nobody happened to do it this way"? The territory shows you the field the researcher actually stands on — and whether it truly hurts there.

**2. The motive of possibility — the researcher's mind 💡**
How was this solution actually conceived? From an observation, a failure-mode diagnosis, an analogy from another field — or trial-and-error dressed up as insight? The test: for every non-trivial design decision, does the paper give a *why* (defensible, falsifiable) or only a *what* ("we adopt...")? Ablations are the window here: do they test the core idea, or perform theater on trivia?

Experiments and benchmarks matter, but they are downstream — merely checking whether the motives were honored. The motives are the essence.

## Grades with teeth

Three dimensions, each S/A/B/C/D, anchored level-by-level so different agents converge:

| Dim | What it measures | S looks like | C/D looks like |
|---|---|---|---|
| **Territory** 🌍 | Is the problem real and important? | Field-acknowledged bottleneck; solving it changes the field's trajectory | A narrative manufactured for the paper (D) |
| **Mind** 💡 | Was the method understood or stumbled upon? | Clear intellectual origin, a *why* for every key design, honest failure boundaries | A trick that happens to work, post-hoc mythologized (C) |
| **Evidence** 🔬 | Is the core claim cashed out? | Every claim keyword has direct evidence; strongest baselines present; failures honest | Conclusions carried by rhetoric and figure magic (C) |

The overall grade is **not** an average — it answers one question: *after stripping the packaging, what is the paper's net contribution to the field?* High Territory + Mind with weak Evidence = promising unfinished work worth following. Solid Evidence with a mundane Mind = useful engineering, skim it.

**And the tone follows the grade.** Truly good papers get explicit, specific praise — stinginess with admiration for great work is as much a failure as stinginess with criticism for bad work. Weak papers get blunt, evidence-anchored takedowns.

## The verdict report

Every run ends in a structured autopsy report — grades up front, then the paper's self-claims, the territory, the mind, a proven/suggested/packaging verdict, and a researcher portrait:

> *"This researcher holds ___ as territory, working with a ___ mind. That — not this one paper — decides whether you should read their next one."*

Every judgment must be anchored to a specific location in the paper (quoted phrase + section/figure). Unsourced qualitative judgments are downgraded to "suspected" — an autopsy that runs on vibes would be just another kind of packaging.

**What does the output actually look like?** See [examples/attention_report.md](examples/attention_report.md) — a full verdict report on *Attention Is All You Need* (real skill output; the analyzed paper's PDF sits in the same folder, so you can verify every claim against the source). The only way to judge a pure-prompt product is its output; this is our answer sheet.

## Install

Copy the repo folder into your agent's skills directory:

```bash
git clone https://github.com/stonebubu/paper-autopsy.git
mkdir -p ~/.agents/skills
cp -r paper-autopsy ~/.agents/skills/paper-autopsy
```

Other agents: copy the folder into their skills directory — Claude Code uses `~/.claude/skills/`, ZCode uses `~/.zcode/skills/` (which takes priority over `~/.agents/skills/`), and project-level installs go into the corresponding project directory.

Then just talk to your agent naturally — any paper (PDF path, arXiv link, or title) plus any reading intent triggers it:

> "帮我看看这篇 https://arxiv.org/abs/xxxx.xxxxx"
> "read this paper and tell me what it really shows"

No configuration, no API keys, no per-paper flags. Triggered = full pipeline.

The skill body is written in Chinese but works fully in English contexts: it triggers on both Chinese and English requests, and reports are always written in the language the user is speaking.

## FAQ

**Why no checklist of "10 packaging tricks to watch for"?**
A strong checklist requires having autopsied many papers. This skill deliberately encodes *judgment* (the dual-motive framework, the grading anchors) instead of *enumeration* — judgment generalizes to packaging shapes it has never seen; checklists only catch the known ones.

**Who are you to grade someone's paper?**
The grades are not a tribunal for authors — they are a decision tool for readers: they compress an autopsy into an actionable "should *I* spend my weekend on this." Every grade is anchored to published criteria and to specific evidence in the paper; disagree with the evidence, not the vibe.

**Does it only work on ML/AI papers?**
It was designed with empirical ML/AI papers as the primary case. The pipeline is domain-agnostic; for theory papers, swap "experimental evidence" for "soundness of proofs and reasonableness of assumptions."

**Will it trash a good paper just to seem deep?**
No — the skill explicitly forbids criticism for its own sake. Recognizing real gold is as important as exposing packaging, and the tone contract rewards specific, justified praise at the top grades.

**Does the skill itself make mistakes?**
Yes. LLMs reading papers hallucinate citations, misjudge baseline strength, and get carried by prose quality; this skill is not immune. The existing mitigations are mandatory evidence anchoring (every judgment tied to a location in the paper, downgraded when it cannot be) and level-by-level anchored grading criteria that suppress drift between agents — but mitigation is not immunity. Which is why the report is always worth checking against the paper, not taking on faith.

**Will newcomers who use it lose the chance to grow their own judgment?**
Judgment doesn't grow in a sterile environment, but it doesn't need three years of being fooled as vaccination either. Every report comes with its evidence locations — which means "why an experienced reader judged this way" is laid out for inspection. Verifying one of its evidence chains *is* a training rep. The real risk isn't lost growth, it's accepting everything on faith; so check the report against the paper, especially where it disagrees with your intuition.

## License

MIT
