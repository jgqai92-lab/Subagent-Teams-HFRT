# HFRT Quick Start Guide

> Get up and running with the Hedge Fund Research Team in 5 minutes.

---

## What is HFRT?

The Hedge Fund Research Team (HFRT) is a multi-agent framework that produces professional-grade equity research on public companies. It uses 14 specialized agents coordinated through a 5-phase workflow.

---

## Prerequisites

1. Claude Code CLI installed
2. Agents created via `/agents` command
3. HFRT rules added to your global CLAUDE.md

---

## Creating Agents

### Step 1: Open Agent Manager

```
/agents
```

### Step 2: Create Each Agent

Create these 14 agents using the definitions in the `agents/` folder:

| Agent | Model | Definition File |
|-------|-------|-----------------|
| @HFRT_Commander | Sonnet | agents/HFRT_Commander.md |
| @Idea_Screener | Sonnet | agents/Idea_Screener.md |
| @Fundamental_Analyst | Opus | agents/Fundamental_Analyst.md |
| @Quantitative_Analyst | Opus | agents/Quantitative_Analyst.md |
| @Sector_Technology | Opus | agents/Sector_Technology.md |
| @Sector_Healthcare | Opus | agents/Sector_Healthcare.md |
| @Sector_Industrials | Opus | agents/Sector_Industrials.md |
| @Sector_Consumer | Opus | agents/Sector_Consumer.md |
| @Sector_Financials | Opus | agents/Sector_Financials.md |
| @Sector_Energy_Materials | Opus | agents/Sector_Energy_Materials.md |
| @Due_Diligence_Officer | Opus | agents/Due_Diligence_Officer.md |
| @Bull_Analyst | Opus | agents/Bull_Analyst.md |
| @Bear_Analyst | Opus | agents/Bear_Analyst.md |
| @Thesis_Synthesizer | Opus | agents/Thesis_Synthesizer.md |

---

## Starting Research

### Basic Command

```
@HFRT_Commander, research [TICKER]
```

**Examples:**
```
@HFRT_Commander, research AAPL
@HFRT_Commander, research NVDA
@HFRT_Commander, analyze Microsoft Corp
```

### What Happens Next

1. **Phase 1:** @Idea_Screener validates the company and routes to sector specialist
2. **Phase 2:** @Fundamental_Analyst, @Sector_*, @Quantitative_Analyst complete research (sequential)
3. **Phase 3:** @Due_Diligence_Officer assesses risks and management
4. **Phase 4:** @Bull_Analyst and @Bear_Analyst develop cases (parallel, isolated)
5. **Phase 5:** @Thesis_Synthesizer produces final investment memo

---

## Project Setup

### For Each Research Project

1. Copy research templates to your project:
```
cp -r "Hedge Fund Research Team/research_template" ./[project]/
```

2. Create coordination folder:
```
mkdir ./[project]/.hfrt
```

3. Start research:
```
@HFRT_Commander, research [TICKER]
```

### Folder Structure

```
[your-project]/
├── research_template/
│   ├── 00_IDEA_SCREEN.md
│   ├── 01_COMPANY_OVERVIEW.md
│   ├── ... (15 templates total)
│   └── 14_INVESTMENT_MEMO_FINAL.md
└── .hfrt/
    ├── PROGRESS.md
    ├── BULL_REVIEW.md
    └── BEAR_REVIEW.md
```

---

## Checking Progress

### View Progress Log

```
Show me .hfrt/PROGRESS.md
```

### Sample Progress Log

```markdown
## Checkpoint Log

| Timestamp | Phase | Step | Agent | Status | Output File |
|-----------|-------|------|-------|--------|-------------|
| 2026-02-05T10:00:00 | 1 | 1.1 | @Idea_Screener | COMPLETE | 00_IDEA_SCREEN.md |
| 2026-02-05T10:30:00 | 2 | 2.1 | @Fundamental_Analyst | COMPLETE | 01_COMPANY_OVERVIEW.md |
| 2026-02-05T11:00:00 | 2 | 2.2 | @Fundamental_Analyst | IN_PROGRESS | 02_BUSINESS_MODEL.md |
```

---

## Resuming Research

If research is interrupted, resume from last checkpoint:

```
@HFRT_Commander, resume research on [TICKER] from Phase [X]
```

**Example:**
```
@HFRT_Commander, resume research on AAPL from Phase 2
```

---

## Emergency Stop

To halt research immediately:

```
HALT HFRT
```

or

```
STOP
```

---

## Output Files

### Final Deliverable

The primary output is `14_INVESTMENT_MEMO_FINAL.md`, which includes:

- Executive summary
- Investment thesis
- Valuation summary
- Bull/bear cases
- Risk assessment
- Conviction score and position sizing

### Key Templates

| Template | Content |
|----------|---------|
| 06_VALUATION.md | DCF, comps, price targets |
| 11_INVESTMENT_THESIS.md | Core thesis statement |
| 12_BULL_CASE.md | Upside scenario |
| 13_BEAR_CASE.md | Downside scenario |
| 14_INVESTMENT_MEMO_FINAL.md | Complete memo |

---

## Understanding Conviction Scores

| Score | Level | Position Size |
|-------|-------|---------------|
| 5 | Very High | 4-6% |
| 4 | High | 2-4% |
| 3 | Medium | 1-2% |
| 2 | Low | 0.5-1% |
| 1 | Very Low | Pass |

---

## Common Commands

| Command | Purpose |
|---------|---------|
| `@HFRT_Commander, research [TICKER]` | Start new research |
| `@HFRT_Commander, resume from Phase [X]` | Continue interrupted research |
| `Show .hfrt/PROGRESS.md` | Check progress |
| `HALT HFRT` | Emergency stop |

---

## Quality Gates

Research cannot proceed until gates pass:

### Gate 1: Research Sufficiency (Phase 2 → 3)
- All research templates (00-06) complete
- All claims cited
- Assumption registry populated

### Gate 2: DD Sufficiency (Phase 3 → 4)
- Management assessment complete
- Risk register populated
- Quality of earnings reviewed

### Gate 3: Thesis Coherence (Phase 5)
- Bull/bear balanced
- Catalyst identified
- Downside quantified

---

## Anti-Hallucination Tags

Every claim must be tagged:

| Tag | Use |
|-----|-----|
| `[SEC-CITE: 10-K FY24, p.23]` | SEC filing reference |
| `[TRANSCRIPT: Q3 2024]` | Earnings call |
| `[ESTIMATE(method)]` | Your estimate with methodology |
| `[CONSENSUS]` | Analyst consensus |
| `[GAP: reason]` | Data not available |

---

## Troubleshooting

### "Gate failed"
Check gate requirements. Return to responsible agent to complete missing work.

### "Missing citations"
Add SEC-CITE, TRANSCRIPT, ESTIMATE, or GAP tags to all claims.

### "Progress lost"
Read .hfrt/PROGRESS.md and resume from last checkpoint.

### "Research seems one-sided"
Verify Bull/Bear isolation. @Thesis_Synthesizer must balance both.

---

## Reference Documents

| Document | Location |
|----------|----------|
| Complete Guide | `HEDGE_FUND_RESEARCH_TEAM_GUIDE.md` |
| CLAUDE.md Rules | `CLAUDE_MD_SECTION.md` |
| Agent Definitions | `agents/` folder |
| Research Templates | `research_template/` folder |
| Analysis Frameworks | `frameworks/` folder |

---

## Next Steps

1. Create agents using `/agents` command
2. Copy HFRT rules to your global CLAUDE.md (see CLAUDE_MD_SECTION.md)
3. Start your first research: `@HFRT_Commander, research [TICKER]`

---

**Need more detail?** See `HEDGE_FUND_RESEARCH_TEAM_GUIDE.md` for complete documentation.
