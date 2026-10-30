# market-analysis

**Español:** [README.es.md](README.es.md)

A Claude Code skill that produces executive-grade market analyses with the rigor of a top-tier strategy consultancy: sourced market sizing, MECE segmentation, drivers and brakes, competitive landscape, opportunities and risks, and an honest recommendation that can be "do not enter" or "not yet". Output is a pyramid-structured report with action titles, an SCQ introduction, a stated confidence level, a "case against" section, and three appendices (key-data table, data not found, open questions). Delivered as a Word document by default.

The skill is written in Spanish. It works for prompts in Spanish or English and for any sector and geography.

## What makes it different

- **Data integrity first.** Every relevant claim is tagged `[HECHO]` (fact, with source and date), `[ESTIMACIÓN]` (third-party estimate, with the author named) or `[INFERENCIA]` (the model's own conclusion, with reasoning). Anything not found in a source consulted during the session is written as `[DATO NO DISPONIBLE]` with a note on what was searched. No invented sources, titles, authors, dates or URLs.
- **Honest by design.** Bad news is stated directly. The recommendation may be negative. Each report includes the strongest case against its own recommendation and the signals that would prove it wrong.
- **Hypotheses before search.** The skill writes 3-5 hypotheses that would answer the central question and then searches to refute them, not to confirm them.
- **Pyramid format.** The executive summary answers the question in its first paragraph. Section titles are full sentences stating conclusions; read in sequence, they tell the whole story. Every figure carries its "so what" for the reader's decision.
- **Guided intake.** The brief is collected one question at a time through Claude Code's modal question dialog (`AskUserQuestion`): market and scope, geographies, who the reader is, what decision depends on the report, the central question, horizon and format. Options adapt to previous answers. Questions already answered in the prompt are skipped.

## Installation

### Claude Code (user-level, all projects)

```bash
git clone https://github.com/Claudio-C/market-analysis-skill.git
cp -r market-analysis-skill/market-analysis ~/.claude/skills/
```

### Claude Code (project-level)

```bash
cp -r market-analysis-skill/market-analysis .claude/skills/
```

### Packaged `.skill` file

The `dist/market-analysis.skill` file in this repository can be imported wherever Claude supports skill packages.

Restart Claude Code after installing. The skill then triggers automatically on requests such as "market study", "market sizing", "should we enter X", "TAM/SAM/SOM", "competitive landscape" or "report for the board", and can be forced with `/market-analysis`.

## Usage

Type a request, or invoke the skill directly:

```
/market-analysis
```

You can also pass a URL as the argument. The skill reads the page first (for example your company's services page) and adapts the intake questions to that business:

```
/market-analysis https://example.com/services
```

The skill then asks, one modal at a time:

| # | Parameter | Example |
|---|---|---|
| 1 | Market and scope | "Cybersecurity: managed services, security hardware, AI products" |
| 2 | Geographies (multi-select) | Global, Europe, Spain, MENA, LATAM |
| 3 | Who the reader is | Chief strategy officer, investor, founder, board |
| 4 | What decision depends on it | Enter or not, pick a segment, value an acquisition |
| 5 | Central question | "Is Spain a sufficient market to start and scale globally?" |
| 6 | Horizon and format | 3-5 years, Word (default) · Markdown |

After the last answer it summarizes the brief and starts: hypotheses, web research with primary sources first, a running key-data table, the pyramid report, and a final review that checks every figure has a source and a date.

## Output structure

```
Title
Introduction (SCQ: Situation, Complication, Question)
Executive summary (answer + recommendation, confidence level, 3-4 key arguments)
1. [Action title on market size and growth]
2. [Action title on segmentation]
3. [Action title on drivers and brakes]
4. [Action title on competition]
5. [Action title on opportunities and risks for the reader]
6. Recommendation and next steps
   Case against this recommendation
   What would have to happen for this recommendation to be wrong
Appendix 1. Key-data table: datum | value | source | date | type | confidence
Appendix 2. Data not available: what was searched and not found
Appendix 3. Open questions before deciding
Sources consulted (with URLs)
```

Word output uses real heading styles, native tables and page numbering. It requires the `docx` skill (bundled with Claude Code's Anthropic skills); without it the report is delivered in Markdown.

## Repository layout

```
market-analysis/
├── SKILL.md                      # the skill: intake, coverage, integrity rules, format, process, delivery
├── references/
│   └── report-template.md        # exact report structure, good/bad title examples, final checklist
└── evals/                        # test prompts (not yet run as a formal benchmark)
    ├── ciberseguridad-espana-nuevo-entrante/prompt.md
    └── no-entrar-mercado-saturado/prompt.md
dist/
└── market-analysis.skill         # packaged skill
```

## Tips

- Research works best when the model can run web searches and, for larger briefs, delegate research by geography or topic to subagents and keep only their verified findings.
- Expect declared gaps. A report with visible `[DATO NO DISPONIBLE]` entries is working as intended; a report with no gaps and no sources is not.
- If the recommendation comes back as "do not enter", read the "case against" section before pushing back: it is written to be the strongest argument for the other side.

## License

MIT. See [LICENSE](LICENSE).
