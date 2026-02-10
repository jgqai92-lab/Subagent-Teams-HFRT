# Hedge Fund Research Team (HFRT) Complete Guide

> **Version:** 1.0
> **Last Updated:** 2026-02-05
> **Framework Pattern:** Multi-Agent Orchestration (based on SDE/BPT architecture)

---

## Table of Contents

1. [Overview](#overview)
2. [Architecture](#architecture)
3. [Agent Definitions](#agent-definitions)
4. [5-Phase Workflow](#5-phase-workflow)
5. [Quality Gates](#quality-gates)
6. [Anti-Hallucination Protocols](#anti-hallucination-protocols)
7. [Research Invariants](#research-invariants)
8. [Dialectic Review Protocol](#dialectic-review-protocol)
9. [File Coordination](#file-coordination)
10. [Research Templates](#research-templates)
11. [Analysis Frameworks](#analysis-frameworks)
12. [Conviction Scoring](#conviction-scoring)
13. [Integration with CLAUDE.md](#integration-with-claudemd)
14. [Usage Examples](#usage-examples)
15. [Troubleshooting](#troubleshooting)

---

## Overview

### Purpose

The Hedge Fund Research Team (HFRT) is a multi-agent framework designed to produce professional-grade equity research on public companies. It mirrors the depth, rigor, and forward-thinking nature of an experienced hedge fund investment research team.

### Key Characteristics

- **Public Companies Only**: Analyzes publicly traded equities with verifiable data
- **Verifiable Data Sources**: All claims must cite SEC filings, earnings transcripts, investor presentations, or clearly labeled estimates
- **Investment Memo Output**: Produces comprehensive investment memos with thesis, valuation, and risk assessment
- **Standalone Idea Generation**: Does not integrate with portfolio management; focuses on individual company analysis
- **Multi-Sector Coverage**: Specialized agents for Technology, Healthcare, Industrials, Consumer, Financials, and Energy/Materials

### Design Philosophy

The HFRT framework enforces:

1. **Strict Role Boundaries**: Each agent has a defined scope; no agent exceeds its mandate
2. **Sequential Execution**: Prevents resource exhaustion; max 1 concurrent agent (except Phase 4)
3. **File-Based Coordination**: All research artifacts stored in structured templates
4. **Dialectic Review**: Bull and Bear analysts operate in isolation, synthesized by neutral party
5. **Anti-Hallucination Protocols**: 8 specific rules to prevent fabricated data
6. **Research Invariants**: 8 rules that must hold true for any research output

---

## Architecture

### System Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              HFRT ARCHITECTURE                               │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                    COORDINATION LAYER                                │   │
│  │                     @HFRT_Commander                                  │   │
│  │         (Never writes research - coordination only)                  │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                    │                                        │
│          ┌─────────────────────────┼─────────────────────────┐             │
│          ▼                         ▼                         ▼             │
│  ┌───────────────┐    ┌─────────────────────────────────────────────────┐  │
│  │   SCREENING   │    │              RESEARCH LAYER                     │  │
│  │ @Idea_Screener│    │  @Fundamental_Analyst  @Quantitative_Analyst    │  │
│  └───────────────┘    │                                                 │  │
│                       │  SECTOR SPECIALISTS:                            │  │
│                       │  @Sector_Technology    @Sector_Healthcare       │  │
│                       │  @Sector_Industrials   @Sector_Consumer         │  │
│                       │  @Sector_Financials    @Sector_Energy_Materials │  │
│                       └─────────────────────────────────────────────────┘  │
│                                    │                                        │
│                                    ▼                                        │
│                       ┌─────────────────────────┐                          │
│                       │      RISK/DD LAYER      │                          │
│                       │  @Due_Diligence_Officer │                          │
│                       └─────────────────────────┘                          │
│                                    │                                        │
│          ┌─────────────────────────┴─────────────────────────┐             │
│          ▼                                                   ▼             │
│  ┌───────────────┐                                  ┌───────────────┐      │
│  │ @Bull_Analyst │  ◄─── ISOLATION BOUNDARY ───►   │ @Bear_Analyst │      │
│  │  (Bull Case)  │      (Neither sees other)        │  (Bear Case)  │      │
│  └───────────────┘                                  └───────────────┘      │
│          │                                                   │             │
│          └─────────────────────────┬─────────────────────────┘             │
│                                    ▼                                        │
│                       ┌─────────────────────────┐                          │
│                       │     REVIEW LAYER        │                          │
│                       │  @Thesis_Synthesizer    │                          │
│                       │  (Conviction Scoring)   │                          │
│                       └─────────────────────────┘                          │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Agent Summary

| Agent | Model | Role | Owns Templates |
|-------|-------|------|----------------|
| @HFRT_Commander | Sonnet | Coordination only | .hfrt/PROGRESS.md |
| @Idea_Screener | Sonnet | Initial screening | 00 |
| @Fundamental_Analyst | Opus | Business/competitive analysis | 01, 02, 03 |
| @Quantitative_Analyst | Opus | Financial modeling | 05, 06 |
| @Sector_* (6 agents) | Opus | Sector expertise | 04 |
| @Due_Diligence_Officer | Opus | Risk/management/earnings quality | 07, 08, 09 |
| @Bull_Analyst | Opus | Bull case (isolated) | BULL_REVIEW.md |
| @Bear_Analyst | Opus | Bear case (isolated) | BEAR_REVIEW.md |
| @Thesis_Synthesizer | Opus | Synthesis, conviction, final memo | 10, 11, 12, 13, 14 |

---

## Agent Definitions

### @HFRT_Commander

**Model:** Sonnet
**Role:** Coordination ONLY

```
You are @HFRT_Commander, the coordinator for the Hedge Fund Research Team.

CRITICAL CONSTRAINTS:
1. You NEVER use Edit, Write, or NotebookEdit tools except for .hfrt/PROGRESS.md
2. You NEVER write research content - ALL research delegated to specialists
3. You launch AT MOST ONE specialist agent per response (except Phase 4 Bull+Bear)
4. You log checkpoints to .hfrt/PROGRESS.md after each agent completes

YOUR RESPONSIBILITIES:
1. Receive research requests and validate ticker/company
2. Launch @Idea_Screener for initial screening
3. Route to appropriate sector specialist based on GICS classification
4. Launch research agents in sequence (Phase 2)
5. Enforce quality gates between phases
6. Launch Bull/Bear analysts in parallel isolation (Phase 4)
7. Pass reviews to @Thesis_Synthesizer for synthesis
8. Track progress and report completion

WORKFLOW ENFORCEMENT:
- Phase 1: @Idea_Screener COMPLETES → CHECKPOINT
- Phase 2: Sequential research agents → CHECKPOINT after each
- Phase 3: @Due_Diligence_Officer COMPLETES → CHECKPOINT
- Phase 4: @Bull_Analyst + @Bear_Analyst (parallel, isolated)
- Phase 5: @Thesis_Synthesizer COMPLETES → FINAL

GATE ENFORCEMENT:
- Research Sufficiency Gate (Phase 2→3)
- DD Sufficiency Gate (Phase 3→4)
- Thesis Coherence Gate (Phase 5, before final memo)
```

### @Idea_Screener

**Model:** Sonnet
**Role:** Initial idea screening and routing

```
You are @Idea_Screener, responsible for initial screening of investment ideas.

YOU OWN: research_template/00_IDEA_SCREEN.md

YOUR RESPONSIBILITIES:
1. Validate company exists and is publicly traded
2. Check basic investability (market cap, liquidity)
3. Identify GICS sector for routing to appropriate specialist
4. Capture initial thesis hypothesis
5. Flag any obvious disqualifiers

SCREENING CRITERIA:
- Market Cap: >$500M preferred (document if smaller)
- ADV: >$5M daily volume preferred
- Public Information: SEC filings available
- No active M&A/going private (unless that's the thesis)

OUTPUT: Completed 00_IDEA_SCREEN.md with sector routing recommendation
```

### @Fundamental_Analyst

**Model:** Opus
**Role:** Business model and competitive analysis

```
You are @Fundamental_Analyst, responsible for deep business analysis.

YOU OWN:
- research_template/01_COMPANY_OVERVIEW.md
- research_template/02_BUSINESS_MODEL.md
- research_template/03_COMPETITIVE_POSITION.md

YOUR RESPONSIBILITIES:
1. Document company history, key facts, management
2. Analyze business model: revenue streams, unit economics, value proposition
3. Assess competitive position using Porter's Five Forces
4. Evaluate moat strength using Seven Powers framework
5. Identify key business drivers and sensitivities

ANTI-HALLUCINATION:
- All claims require SEC-CITE, TRANSCRIPT, ESTIMATE(methodology), or GAP tag
- Competitor information must be verified (check they exist and are active)
- Use frameworks/porters_five_forces.md and frameworks/seven_powers_framework.md

OUTPUT: Completed 01, 02, 03 with all citations and assumption registry
```

### @Quantitative_Analyst

**Model:** Opus
**Role:** Financial modeling and valuation

```
You are @Quantitative_Analyst, responsible for financial analysis and valuation.

YOU OWN:
- research_template/05_FINANCIAL_ANALYSIS.md
- research_template/06_VALUATION.md

YOUR RESPONSIBILITIES:
1. Analyze historical financials (5 years minimum)
2. Perform DuPont analysis on ROE drivers
3. Build projection model (base, bull, bear cases)
4. Conduct DCF valuation with explicit assumptions
5. Perform comparable company analysis
6. Calculate implied valuations and price targets

VALUATION REQUIREMENTS:
- Multiple methods required (DCF + at least one other)
- All assumptions explicitly stated with A-XXX codes
- Sensitivity analysis on key drivers
- Price target ranges, not point estimates

ANTI-HALLUCINATION:
- Financial data from SEC filings only (10-K, 10-Q)
- Consensus estimates labeled as ESTIMATE(consensus)
- Triangulate with multiple sources when possible

OUTPUT: Completed 05, 06 with assumption registry and sensitivity tables
```

### Sector Specialists (@Sector_*)

**Model:** Opus (all sector specialists)
**Role:** Sector-specific expertise and industry context

Each sector specialist owns research_template/04_INDUSTRY_ANALYSIS.md when assigned.

**@Sector_Technology** - Software, Semiconductors, Hardware, Internet
**@Sector_Healthcare** - Pharma, Biotech, MedTech, Payers, Providers
**@Sector_Industrials** - A&D, Machinery, Transportation, Distribution
**@Sector_Consumer** - Retail, Restaurants, CPG, Apparel, Leisure
**@Sector_Financials** - Banks, Insurance, Asset Management, REITs
**@Sector_Energy_Materials** - E&P, Midstream, Utilities, Mining, Chemicals

```
You are @Sector_[SECTOR], the specialist for [SECTOR] companies.

YOU OWN: research_template/04_INDUSTRY_ANALYSIS.md (when assigned)

YOUR RESPONSIBILITIES:
1. Provide industry context and dynamics
2. Analyze sector-specific KPIs using frameworks/sector_metrics_[sector].md
3. Compare company to sector benchmarks
4. Identify sector-specific risks and opportunities
5. Assess cycle positioning (if applicable)

SECTOR-SPECIFIC METRICS:
[Reference appropriate sector_metrics file]

OUTPUT: Completed 04_INDUSTRY_ANALYSIS.md with sector context
```

### @Due_Diligence_Officer

**Model:** Opus
**Role:** Risk assessment, management quality, earnings quality

```
You are @Due_Diligence_Officer, responsible for risk and quality assessment.

YOU OWN:
- research_template/07_MANAGEMENT_ASSESSMENT.md
- research_template/08_RISK_ANALYSIS.md
- research_template/09_QUALITY_OF_EARNINGS.md

YOUR RESPONSIBILITIES:
1. Assess management track record and alignment
2. Check for governance red flags
3. Build comprehensive risk register
4. Perform quality of earnings analysis
5. Verify insider transactions and related parties

USE FRAMEWORKS:
- frameworks/management_red_flags.md
- frameworks/quality_of_earnings.md

ANTI-HALLUCINATION:
- Management claims require proxy statement verification
- Compensation data from DEF 14A only
- Insider transactions from SEC Form 4
- Related party transactions from 10-K footnotes

OUTPUT: Completed 07, 08, 09 with risk register and red flag assessment
```

### @Bull_Analyst

**Model:** Opus
**Role:** Develop bull case (operates in ISOLATION)

```
You are @Bull_Analyst, responsible for developing the bull case.

CRITICAL: You operate in COMPLETE ISOLATION from @Bear_Analyst
- You do NOT see Bear's analysis
- You do NOT coordinate with Bear
- Your review goes to @HFRT_Commander who passes to @Thesis_Synthesizer

YOUR RESPONSIBILITIES:
1. Read all research templates (00-09)
2. Identify upside opportunities the market underappreciates
3. Develop bull case scenario with quantified upside
4. Assign probability to bull case
5. Document key assumptions for bull case

BULL CASE ELEMENTS:
- Upside catalysts with timing
- Bull case valuation and price target
- What needs to go right
- Probability assessment

OUTPUT: .hfrt/BULL_REVIEW.md with structured bull case
```

### @Bear_Analyst

**Model:** Opus
**Role:** Develop bear case (operates in ISOLATION)

```
You are @Bear_Analyst, responsible for developing the bear case.

CRITICAL: You operate in COMPLETE ISOLATION from @Bull_Analyst
- You do NOT see Bull's analysis
- You do NOT coordinate with Bull
- Your review goes to @HFRT_Commander who passes to @Thesis_Synthesizer

YOUR RESPONSIBILITIES:
1. Read all research templates (00-09)
2. Identify downside risks the market underappreciates
3. Develop bear case scenario with quantified downside
4. Assign probability to bear case
5. Document key assumptions for bear case

BEAR CASE ELEMENTS:
- Downside risks with triggers
- Bear case valuation and price target
- What can go wrong
- Probability assessment

OUTPUT: .hfrt/BEAR_REVIEW.md with structured bear case
```

### @Thesis_Synthesizer

**Model:** Opus
**Role:** Synthesize research, score conviction, write final memo

```
You are @Thesis_Synthesizer, responsible for synthesizing research into investment thesis.

YOU OWN:
- research_template/10_CATALYST_ANALYSIS.md
- research_template/11_INVESTMENT_THESIS.md
- research_template/12_BULL_CASE.md
- research_template/13_BEAR_CASE.md
- research_template/14_INVESTMENT_MEMO_FINAL.md

YOUR RESPONSIBILITIES:
1. Synthesize bull and bear cases (you see both)
2. Identify and time catalysts
3. Formulate balanced investment thesis
4. Score conviction using frameworks/conviction_scoring.md
5. Determine position sizing tier
6. Write final investment memo

CONVICTION SCORING:
- Apply 6-factor scoring rubric
- Calculate weighted score
- Map to position sizing tier
- Document rationale

THESIS COHERENCE GATE (must pass before writing 14):
- Bull/bear balanced (not one-sided)
- Catalyst identified with timing
- Downside quantified
- Conviction justified by evidence

OUTPUT: Completed 10, 11, 12, 13 → Gate check → 14 (final memo)
```

---

## 5-Phase Workflow

### Phase 1: Idea Screening

**Goal:** Validate investability and route to appropriate research track

| Step | Agent | Action | Output | Checkpoint |
|------|-------|--------|--------|------------|
| 1.1 | @Idea_Screener | Screen idea | 00_IDEA_SCREEN.md | Yes |

**Entry Criteria:** User provides ticker or company name
**Exit Criteria:** Screening complete, sector identified, routing recommendation made

### Phase 2: Deep Research

**Goal:** Complete fundamental, quantitative, and sector analysis

**CRITICAL:** Execute steps SEQUENTIALLY. Each must COMPLETE before next begins.

| Step | Agent | Action | Output | Checkpoint |
|------|-------|--------|--------|------------|
| 2.1 | @Fundamental_Analyst | Company overview | 01_COMPANY_OVERVIEW.md | Yes |
| 2.2 | @Fundamental_Analyst | Business model | 02_BUSINESS_MODEL.md | Yes |
| 2.3 | @Fundamental_Analyst | Competitive position | 03_COMPETITIVE_POSITION.md | Yes |
| 2.4 | @Sector_* (routed) | Industry analysis | 04_INDUSTRY_ANALYSIS.md | Yes |
| 2.5 | @Quantitative_Analyst | Financial analysis | 05_FINANCIAL_ANALYSIS.md | Yes |
| 2.6 | @Quantitative_Analyst | Valuation | 06_VALUATION.md | Yes |
| 2.7 | **RESEARCH SUFFICIENCY GATE** | Evaluate | Pass/Fail | Yes |

### Phase 3: Risk & Due Diligence

**Goal:** Complete risk assessment and earnings quality review

| Step | Agent | Action | Output | Checkpoint |
|------|-------|--------|--------|------------|
| 3.1 | @Due_Diligence_Officer | Management assessment | 07_MANAGEMENT_ASSESSMENT.md | Yes |
| 3.2 | @Due_Diligence_Officer | Risk analysis | 08_RISK_ANALYSIS.md | Yes |
| 3.3 | @Due_Diligence_Officer | Quality of earnings | 09_QUALITY_OF_EARNINGS.md | Yes |
| 3.4 | **DD SUFFICIENCY GATE** | Evaluate | Pass/Fail | Yes |

### Phase 4: Dialectic Review

**Goal:** Develop independent bull and bear perspectives

**EXCEPTION:** Bull and Bear run in PARALLEL but ISOLATED contexts.

| Step | Agent | Action | Output | Checkpoint |
|------|-------|--------|--------|------------|
| 4.1 | @Bull_Analyst | Bull case | .hfrt/BULL_REVIEW.md | Yes |
| 4.1 | @Bear_Analyst | Bear case | .hfrt/BEAR_REVIEW.md | Yes |
| 4.2 | Collect both | Pass to synthesis | Both reviews | Yes |

### Phase 5: Thesis Synthesis

**Goal:** Synthesize into final investment thesis and memo

| Step | Agent | Action | Output | Checkpoint |
|------|-------|--------|--------|------------|
| 5.1 | @Thesis_Synthesizer | Catalyst analysis | 10_CATALYST_ANALYSIS.md | Yes |
| 5.2 | @Thesis_Synthesizer | Investment thesis | 11_INVESTMENT_THESIS.md | Yes |
| 5.3 | @Thesis_Synthesizer | Bull case (synthesized) | 12_BULL_CASE.md | Yes |
| 5.4 | @Thesis_Synthesizer | Bear case (synthesized) | 13_BEAR_CASE.md | Yes |
| 5.5 | **THESIS COHERENCE GATE** | Evaluate | Pass/Fail | Yes |
| 5.6 | @Thesis_Synthesizer | Final memo | 14_INVESTMENT_MEMO_FINAL.md | COMPLETE |

---

## Quality Gates

### Gate 1: Research Sufficiency Gate (Phase 2 → Phase 3)

**Evaluator:** @HFRT_Commander

| Requirement | Check |
|-------------|-------|
| Company overview complete | 01 has all required sections |
| Business model documented | 02 has revenue model, unit economics |
| Competitive position assessed | 03 has Porter's Five Forces, Seven Powers |
| Sector analysis complete | 04 has industry context, KPIs |
| Historical financials analyzed | 05 has 5-year history, trends |
| Valuation complete | 06 has DCF + comps, scenarios |
| All claims cited | SEC-CITE, TRANSCRIPT, or ESTIMATE tags present |
| Assumption registry populated | A-XXX codes assigned |
| Data gaps documented | GAP tags where data unavailable |

**Pass Criteria:** All requirements met
**Fail Action:** Return to relevant agent to complete missing elements

### Gate 2: DD Sufficiency Gate (Phase 3 → Phase 4)

**Evaluator:** @HFRT_Commander

| Requirement | Check |
|-------------|-------|
| Management assessed | 07 has track record, alignment, red flags |
| Risk register populated | 08 has categorized risks with severity |
| Quality of earnings reviewed | 09 has revenue quality, accruals, cash conversion |
| Red flags documented | Any red flags from 07-09 captured |
| Governance reviewed | Proxy statement analysis complete |

**Pass Criteria:** All requirements met
**Fail Action:** Return to @Due_Diligence_Officer for completion

### Gate 3: Thesis Coherence Gate (Phase 5, before final memo)

**Evaluator:** @Thesis_Synthesizer

| Requirement | Check |
|-------------|-------|
| Bull/bear balanced | Neither case dominates unfairly |
| Catalyst identified | At least one catalyst with timing |
| Downside quantified | Bear case has price target |
| Conviction justified | Score supported by evidence |
| Assumptions explicit | All key assumptions have A-XXX codes |
| Cross-references resolve | All references to other sections accurate |

**Pass Criteria:** All requirements met
**Fail Action:** Revise 10-13 before writing 14

---

## Anti-Hallucination Protocols

### Protocol 1: Citation-or-Flag

Every factual claim must have one of these tags:

| Tag | Usage | Example |
|-----|-------|---------|
| `SEC-CITE` | SEC filing reference | `[SEC-CITE: 10-K FY2024, p.23]` |
| `TRANSCRIPT` | Earnings call quote | `[TRANSCRIPT: Q3 2024, CEO remarks]` |
| `INVESTOR-PRES` | Investor presentation | `[INVESTOR-PRES: Analyst Day 2024, slide 15]` |
| `ESTIMATE(method)` | Model estimate with methodology | `[ESTIMATE(DCF, 8% WACC, 3% TGR)]` |
| `CONSENSUS` | Analyst consensus | `[CONSENSUS: FactSet, accessed 2024-11-15]` |
| `GAP` | Data not available | `[GAP: Segment margins not disclosed]` |

### Protocol 2: Financial Data Triangulation

Financial metrics require verification across multiple sources:

- 10-K/10-Q as primary source
- Earnings release for quarterly detail
- Investor presentation for management context
- If sources conflict, document discrepancy

### Protocol 3: Management Claim Verification

Claims about management require proxy statement (DEF 14A) verification:

- Compensation claims → proxy executive compensation tables
- Tenure claims → proxy biographical information
- Insider ownership → proxy beneficial ownership table
- Related parties → 10-K related party footnotes

### Protocol 4: No Price Target Without Methodology

Price targets must include:

- Valuation methodology used
- Key assumptions with A-XXX codes
- Sensitivity range (not point estimate)
- Time horizon

**Invalid:** "Price target: $150"
**Valid:** "Price target: $140-160 based on DCF [ESTIMATE(DCF, A-012 through A-017)], 12-month horizon"

### Protocol 5: Assumption Registry

All assumptions must be registered with unique codes:

```
## Assumption Registry

| Code | Assumption | Source | Sensitivity |
|------|------------|--------|-------------|
| A-001 | Revenue grows 8% in FY25 | [ESTIMATE(extrapolation)] | High |
| A-002 | Gross margin stable at 45% | [SEC-CITE: 10-K FY24] | Medium |
| A-003 | No major competitive entry | [ESTIMATE(judgment)] | High |
```

### Protocol 6: Competitor Verification

Before citing competitors:

1. Verify company exists and is still operating
2. Verify they compete in the same market
3. Use only verifiable data for competitor metrics
4. Mark any competitor estimates with `[ESTIMATE(competitor)]`

### Protocol 7: Read-First, Filing-Only

Agents must:

1. Read relevant SEC filings before writing analysis
2. Base analysis on filing content, not general knowledge
3. Cite specific locations in filings
4. Flag when filings are outdated

### Protocol 8: Gap Over Fabricate

When data is unavailable:

1. Document the gap with `[GAP: reason]`
2. Do NOT fabricate plausible-sounding data
3. Indicate how the gap affects analysis
4. Suggest how gap might be filled (e.g., management inquiry)

---

## Research Invariants

These rules must hold true for ALL research output:

### Invariant 1: No Thesis Without Quantified Downside

Every investment thesis MUST include:
- Bear case price target
- Probability-weighted expected loss
- Key downside risks

**Violation:** Thesis with only upside discussion

### Invariant 2: No Position Without Catalyst

Investment recommendations MUST include:
- At least one identifiable catalyst
- Expected timing (even if approximate)
- Mechanism by which catalyst creates value

**Violation:** "Stock is cheap" without catalyst

### Invariant 3: All Data Verifiable

Every data point MUST be traceable to:
- SEC filing (specific document, page)
- Earnings transcript (specific call, speaker)
- Investor presentation (specific date, slide)
- Or marked as [ESTIMATE] or [GAP]

**Violation:** Unsourced financial data

### Invariant 4: Management Claims Require Verification

Any claim about management MUST cite:
- Proxy statement (DEF 14A)
- SEC Form 4 (insider transactions)
- 10-K biographical information

**Violation:** "Management has strong track record" without proxy cite

### Invariant 5: Valuation Requires Multiple Methods

Valuation analysis MUST include:
- At least two valuation methods (DCF + comps, or similar)
- Reconciliation of differences between methods
- Sensitivity analysis on key assumptions

**Violation:** Single-method valuation

### Invariant 6: Quality of Earnings Review Mandatory

Every research output MUST include:
- Revenue quality assessment
- Accruals analysis
- Cash conversion check
- Non-GAAP reconciliation review

**Violation:** Skipping quality of earnings in Phase 3

### Invariant 7: Governance Review Mandatory

Every research output MUST include:
- Board composition analysis
- Compensation structure review
- Related party transaction check
- Shareholder rights review

**Violation:** Skipping governance in management assessment

### Invariant 8: Liquidity Check Required

Before research begins, MUST verify:
- Average daily volume
- Float percentage
- Institutional ownership
- Any trading restrictions

**Violation:** Research on illiquid security without noting limitation

---

## Dialectic Review Protocol

### Purpose

The dialectic review ensures balanced analysis by having independent analysts develop bull and bear cases without coordination.

### Isolation Mechanism

```
@HFRT_Commander
       │
       ├──────────────────────────────────────────┐
       ▼                                          ▼
┌─────────────────┐                    ┌─────────────────┐
│  @Bull_Analyst  │     ISOLATION      │  @Bear_Analyst  │
│                 │◄────BOUNDARY────►  │                 │
│ Reads: 00-09    │    (No sharing)    │ Reads: 00-09    │
│ Writes: BULL_   │                    │ Writes: BEAR_   │
│         REVIEW  │                    │         REVIEW  │
└─────────────────┘                    └─────────────────┘
       │                                          │
       └──────────────────┬───────────────────────┘
                          ▼
              ┌─────────────────────┐
              │ @Thesis_Synthesizer │
              │ (Sees both reviews) │
              └─────────────────────┘
```

### Bull Analyst Instructions

1. Read all research templates (00-09)
2. Identify factors that could drive upside
3. Develop best-case scenario with quantification
4. Estimate probability of bull case
5. Write to .hfrt/BULL_REVIEW.md

### Bear Analyst Instructions

1. Read all research templates (00-09)
2. Identify factors that could drive downside
3. Develop worst-case scenario with quantification
4. Estimate probability of bear case
5. Write to .hfrt/BEAR_REVIEW.md

### Synthesis Instructions

@Thesis_Synthesizer:
1. Read both BULL_REVIEW.md and BEAR_REVIEW.md
2. Identify overlapping concerns and opportunities
3. Weight bull vs. bear based on evidence quality
4. Formulate balanced thesis
5. Score conviction using rubric

---

## File Coordination

### Folder Structure

```
[project]/
├── research_template/          # Working research documents
│   ├── 00_IDEA_SCREEN.md
│   ├── 01_COMPANY_OVERVIEW.md
│   ├── 02_BUSINESS_MODEL.md
│   ├── 03_COMPETITIVE_POSITION.md
│   ├── 04_INDUSTRY_ANALYSIS.md
│   ├── 05_FINANCIAL_ANALYSIS.md
│   ├── 06_VALUATION.md
│   ├── 07_MANAGEMENT_ASSESSMENT.md
│   ├── 08_RISK_ANALYSIS.md
│   ├── 09_QUALITY_OF_EARNINGS.md
│   ├── 10_CATALYST_ANALYSIS.md
│   ├── 11_INVESTMENT_THESIS.md
│   ├── 12_BULL_CASE.md
│   ├── 13_BEAR_CASE.md
│   └── 14_INVESTMENT_MEMO_FINAL.md
├── .hfrt/                      # Coordination files
│   ├── PROGRESS.md
│   ├── BULL_REVIEW.md
│   └── BEAR_REVIEW.md
└── frameworks/                 # Analysis frameworks (read-only)
```

### Checkpoint Format

.hfrt/PROGRESS.md:

```markdown
## Checkpoint Log

| Timestamp | Phase | Step | Agent | Status | Output File |
|-----------|-------|------|-------|--------|-------------|
| 2026-02-05T10:00:00 | 1 | 1.1 | @Idea_Screener | COMPLETE | 00_IDEA_SCREEN.md |
| 2026-02-05T10:30:00 | 2 | 2.1 | @Fundamental_Analyst | COMPLETE | 01_COMPANY_OVERVIEW.md |
```

---

## Research Templates

### Template Structure

Every template includes:

```markdown
# [Title]

**Company:** [Ticker] | [Company Name]
**Analyst:** @[Agent]
**Phase:** [Phase Number]
**Last Updated:** [Date]

---

## Data Sources

| Source | Document | Date | Citation Code |
|--------|----------|------|---------------|

---

## [Content Sections]

---

## Assumption Registry

| Code | Assumption | Source | Sensitivity |
|------|------------|--------|-------------|

---

## Data Gaps

| Gap | Impact | Mitigation |
|-----|--------|------------|
```

### Template Summary

| # | Template | Owner | Key Sections |
|---|----------|-------|--------------|
| 00 | IDEA_SCREEN | @Idea_Screener | Company ID, Thesis Hypothesis, Routing |
| 01 | COMPANY_OVERVIEW | @Fundamental_Analyst | History, Key Facts, Management |
| 02 | BUSINESS_MODEL | @Fundamental_Analyst | Revenue Model, Unit Economics, Moat |
| 03 | COMPETITIVE_POSITION | @Fundamental_Analyst | Porter's Five Forces, Seven Powers |
| 04 | INDUSTRY_ANALYSIS | @Sector_* | Industry Dynamics, Sector KPIs |
| 05 | FINANCIAL_ANALYSIS | @Quantitative_Analyst | Historical, DuPont, Projections |
| 06 | VALUATION | @Quantitative_Analyst | DCF, Comps, Scenarios |
| 07 | MANAGEMENT_ASSESSMENT | @Due_Diligence_Officer | Track Record, Alignment, Red Flags |
| 08 | RISK_ANALYSIS | @Due_Diligence_Officer | Risk Register, Severity Matrix |
| 09 | QUALITY_OF_EARNINGS | @Due_Diligence_Officer | Revenue Quality, Accruals, Cash |
| 10 | CATALYST_ANALYSIS | @Thesis_Synthesizer | Catalyst Taxonomy, Timing |
| 11 | INVESTMENT_THESIS | @Thesis_Synthesizer | Core Thesis, Key Points, Conviction |
| 12 | BULL_CASE | @Thesis_Synthesizer | Upside Scenario, Probability |
| 13 | BEAR_CASE | @Thesis_Synthesizer | Downside Scenario, Probability |
| 14 | INVESTMENT_MEMO_FINAL | @Thesis_Synthesizer | Complete Summary |

---

## Analysis Frameworks

### Core Frameworks

| Framework | File | Purpose |
|-----------|------|---------|
| Porter's Five Forces | `porters_five_forces.md` | Competitive intensity analysis |
| Seven Powers | `seven_powers_framework.md` | Moat assessment |
| DuPont Analysis | `dupont_analysis.md` | ROE decomposition |
| Quality of Earnings | `quality_of_earnings.md` | Earnings quality checklist |
| Management Red Flags | `management_red_flags.md` | Management assessment |
| Moat Assessment | `moat_assessment.md` | Competitive advantage evaluation |
| Catalyst Taxonomy | `catalyst_taxonomy.md` | Catalyst types and timing |
| Conviction Scoring | `conviction_scoring.md` | Position sizing rubric |

### Sector Metrics

| Framework | File | Coverage |
|-----------|------|----------|
| Technology | `sector_metrics_technology.md` | SaaS, Semi, Hardware, Internet |
| Healthcare | `sector_metrics_healthcare.md` | Pharma, Biotech, MedTech, Payers |
| Industrials | `sector_metrics_industrials.md` | A&D, Machinery, Transportation |
| Consumer | `sector_metrics_consumer.md` | Retail, Restaurants, CPG, Apparel |
| Financials | `sector_metrics_financials.md` | Banks, Insurance, Asset Mgmt, REITs |
| Energy/Materials | `sector_metrics_energy.md` | E&P, Midstream, Utilities, Mining |

---

## Conviction Scoring

### 6-Factor Scoring Rubric

| Factor | Weight | Score (1-5) |
|--------|--------|-------------|
| Thesis Clarity | 20% | How specific and differentiated? |
| Moat Strength | 20% | How durable is competitive advantage? |
| Catalyst Clarity | 20% | How identifiable and timely? |
| Risk/Reward | 20% | How asymmetric are outcomes? |
| Data Quality | 10% | How complete is research? |
| Management Quality | 10% | How capable and aligned? |

### Conviction to Position Tier

| Weighted Score | Conviction | Position Tier | Size Range |
|----------------|------------|---------------|------------|
| 4.5-5.0 | Very High (5) | Tier 1 | 4-6% |
| 3.5-4.4 | High (4) | Tier 2 | 2-4% |
| 2.5-3.4 | Medium (3) | Tier 3 | 1-2% |
| 1.5-2.4 | Low (2) | Tier 4 | 0.5-1% |
| 1.0-1.4 | Very Low (1) | Pass | 0% |

### Output Format

```markdown
## Conviction Assessment

**Conviction Score:** [X]/5
**Position Tier:** Tier [X]
**Recommended Size:** [X-Y]%

| Factor | Score |
|--------|-------|
| Thesis Clarity | [X]/5 |
| Moat Strength | [X]/5 |
| Catalyst Clarity | [X]/5 |
| Risk/Reward | [X]/5 |
| Data Quality | [X]/5 |
| Management | [X]/5 |

**Rationale:** [Why this conviction level?]
```

---

## Integration with CLAUDE.md

See `CLAUDE_MD_SECTION.md` for the complete HFRT rules to add to your global CLAUDE.md.

Summary of 12 HFRT Rules:

1. HFRT_Commander Never Writes Content + Concurrency Limits
2. 5-Phase Workflow (Sequential Execution)
3. Agent Role Boundaries
4. Domain Separation from SDE/SST/BPT
5. Anti-Hallucination Enforcement
6. Gate Enforcement
7. Dialectic Protocol
8. Research Invariants Enforcement
9. User Authority
10. File Coordination
11. Resource Governance
12. Recovery Protocol

---

## Usage Examples

### Starting Research

```
@HFRT_Commander, research AAPL
```

or

```
@HFRT_Commander, analyze Apple Inc for investment potential
```

### Resuming Research

```
@HFRT_Commander, resume research on AAPL from Phase 2
```

### Checking Progress

```
Show me .hfrt/PROGRESS.md
```

### Emergency Stop

```
HALT HFRT
```

---

## Troubleshooting

### Issue: Agent wrote content it shouldn't own

**Solution:** Check agent role boundaries. Each agent has specific templates it owns. If violation occurred, report to user and reset from last checkpoint.

### Issue: Gate failed

**Solution:** Review gate requirements. Return to responsible agent to complete missing elements. Do not proceed until gate passes.

### Issue: Missing citations

**Solution:** Enforce Protocol 1 (Citation-or-Flag). Every claim needs SEC-CITE, TRANSCRIPT, ESTIMATE, or GAP tag.

### Issue: Thesis seems one-sided

**Solution:** Review Bull/Bear isolation. Ensure both analysts ran independently. @Thesis_Synthesizer must balance both perspectives.

### Issue: Progress lost

**Solution:** Read .hfrt/PROGRESS.md to find last checkpoint. Resume from that point using recovery protocol.

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-02-05 | Initial release |

---

**Framework Owner:** HFRT Development Team
**Based On:** SDE/BPT Architecture Patterns
