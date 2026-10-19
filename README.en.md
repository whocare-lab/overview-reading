# overview-reading | Literature Synthesis for Qualitative Management Research

[繁體中文](README.md) ｜ **English**

A Claude skill (skill name: `lit-synthesis`) for graduate students in management doing qualitative and case-study research. It integrates several closely read papers into an argued literature review and states research gaps concretely.

## Use it after close-reading

```
English paper (PDF) ──► close-reading (one paper) ──► reading card + literature matrix ──► lit-synthesis (many papers) ──► concept matrix, argument outline, research gaps, draft review
```

The single-paper skill lives in [whocare-lab/close-reading](https://github.com/whocare-lab/close-reading). lit-synthesis works best on its reading cards and `matrix.csv`; given only PDFs, it reads each paper first.

## Outputs

| Output | What it contains |
|---|---|
| **A. Concept matrix** | Source list, coverage check (period, method, context, level of analysis, theory, strength of evidence), papers × concepts matrix with page numbers, patterns |
| **B. Argument outline** | Funnel-shaped sections: one claim per section, topic sentence and sources per paragraph, transitions; Mermaid literature map and suggested reading |
| **C. Research gap statement** | Each gap typed (confusion, neglect, application, problematization), backed by matrix evidence, stating what is missing, why it matters and how your study addresses it |
| **D. Draft review** (on request) | Concept-centric paragraphs with APA 7th citations and a reference list |

Trigger with "synthesize these papers", "write my literature review", "identify research gaps".

## How it differs from "ask an AI to write my literature review"
- **Concept-centric**, never author-by-author.
- **At least two sources per synthesized claim**, or labeled single-source; disagreements must be explained.
- **Cites only the papers you supplied**; anything else goes to suggested reading.
- **No empty gap statements** without evidence.
- **Thin-corpus warnings**: under 5 papers is preliminary; under 3, no definitive gaps.
- **`check_citations.py`**: in-text citations vs. reference list, cited works without a reading card, APA 7 "et al." usage; English and Chinese citations.
- **`verify_quotes.py`**: checks direct quotes against several PDFs at once and reports the source paper and page.

## Installation
1. Fill in `lit-synthesis/references/my-research.md` (or copy the one from close-reading).
2. **claude.ai**: upload `dist/lit-synthesis.zip` in the Skills section of settings.
3. **Claude Code**:
```bash
git clone https://github.com/whocare-lab/overview-reading.git
cp -R overview-reading/lit-synthesis ~/.claude/skills/
pip install pypdfium2
```

## Example
`examples/input/` holds two reading cards from close-reading (Adner, 2017; Pidun et al., 2021); `examples/output/` holds the synthesis. The skill raised the thin-corpus warning, found zero empirical studies and no non-profit or education context, identified one disagreement and one shared assumption, and its draft passed both the citation check and the quote check. Two papers are only a demonstration.

## Notes
Outputs are study aids, not submittable work: rewrite drafts in your own words and verify every citation. "No study has examined…" is a strong claim; the skill can only judge from the papers you supplied.

MIT License.
