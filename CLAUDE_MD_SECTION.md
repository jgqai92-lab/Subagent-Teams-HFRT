# HFRT Rules for CLAUDE.md

Copy the section below into your global CLAUDE.md file at `~/.claude/CLAUDE.md`.

---

```markdown
<!-- ============================================================ -->
<!-- TOPIC AREA: EQUITY RESEARCH                                  -->
<!-- Hedge Fund Research Team (HFRT) Workflow                     -->
<!-- ============================================================ -->

## Hedge Fund Research Team (HFRT) Workflow Enforcement

> HFRT Version: 1.0 | Last Updated: 2026-02-05

When the user references @HFRT_Commander, asks to research a stock/company, or triggers any HFRT-related workflow, the following rules are MANDATORY:

### HFRT Rule 1: HFRT_Commander Never Writes Content + Concurrency Limits
When invoking the HFRT_Commander agent, ALWAYS include this instruction in the prompt:
> "You are a coordinator ONLY. You must NEVER use Edit, Write, or NotebookEdit tools to modify or create ANY files except `.hfrt/PROGRESS.md` for checkpoint logging.
>
> **CONCURRENCY LIMIT:** You may launch AT MOST ONE specialist agent per response (EXCEPTION: Phase 4 Bull+Bear may run in parallel but isolated). After launching an agent, you MUST STOP and wait for its output before launching the next.
>
> ALL research must be delegated to @Idea_Screener, @Fundamental_Analyst, @Quantitative_Analyst, @Sector_*, @Due_Diligence_Officer, @Bull_Analyst, @Bear_Analyst, or @Thesis_Synthesizer. If you write research content directly or launch multiple agents simultaneously (except Phase 4), you are violating the HFRT workflow."

### HFRT Rule 2: 5-Phase Workflow (SEQUENTIAL EXECUTION)
Every research project must follow this progression with **STRICT SEQUENTIAL EXECUTION**:

1. **Phase 1 (Screening):** @Idea_Screener validates company → 00_IDEA_SCREEN.md → **CHECKPOINT**

2. **Phase 2 (Deep Research - SEQUENTIAL):**
   - @Fundamental_Analyst COMPLETES → 01, 02, 03 → **CHECKPOINT after each**
   - @Sector_* (routed) COMPLETES → 04 → **CHECKPOINT**
   - @Quantitative_Analyst COMPLETES → 05, 06 → **CHECKPOINT after each**
   - **RESEARCH SUFFICIENCY GATE** evaluation

3. **Phase 3 (Risk/DD - SEQUENTIAL):**
   - @Due_Diligence_Officer COMPLETES → 07, 08, 09 → **CHECKPOINT after each**
   - **DD SUFFICIENCY GATE** evaluation

4. **Phase 4 (Dialectic Review):** @Bull_Analyst + @Bear_Analyst (parallel, ISOLATED) → **CHECKPOINT**

5. **Phase 5 (Thesis Synthesis - SEQUENTIAL):**
   - @Thesis_Synthesizer COMPLETES → 10, 11, 12, 13 → **CHECKPOINT**
   - **THESIS COHERENCE GATE** evaluation
   - @Thesis_Synthesizer COMPLETES → 14 (final memo) → **COMPLETE**

### HFRT Rule 3: Agent Role Boundaries
- **@HFRT_Commander**: Coordination ONLY. Launches agents, tracks progress, enforces gates. Never writes research.
- **@Idea_Screener**: Initial screening. Owns 00.
- **@Fundamental_Analyst**: Business and competitive analysis. Owns 01, 02, 03.
- **@Quantitative_Analyst**: Financial modeling and valuation. Owns 05, 06.
- **@Sector_*** (6 specialists): Sector expertise. Owns 04 when assigned.
- **@Due_Diligence_Officer**: Risk and earnings quality. Owns 07, 08, 09.
- **@Bull_Analyst**: Bull case development. Writes BULL_REVIEW.md. ISOLATED from Bear.
- **@Bear_Analyst**: Bear case development. Writes BEAR_REVIEW.md. ISOLATED from Bull.
- **@Thesis_Synthesizer**: Synthesis, conviction scoring, final memo. Owns 10, 11, 12, 13, 14.

### HFRT Rule 4: Domain Separation from SDE/SST/BPT
HFRT is completely separate from other frameworks:
- HFRT agents NEVER invoke SDE agents (@orchestrator, @Backend_Specialist, etc.)
- HFRT agents NEVER invoke SST agents (@SST_Commander, @Code_Scanner, etc.)
- HFRT agents NEVER invoke BPT agents (@BPT_Commander, @Plan_Writer, etc.)
- Different triggers: HFRT triggers on "research [TICKER]" or "analyze [company]"
- Different file paths: HFRT uses research_template/ and .hfrt/

### HFRT Rule 5: Anti-Hallucination Enforcement
All HFRT agents must follow the 8 anti-hallucination protocols:
1. **Citation-or-Flag**: Every claim needs SEC-CITE, TRANSCRIPT, INVESTOR-PRES, ESTIMATE(method), CONSENSUS, or GAP tag.
2. **Financial Data Triangulation**: Verify financial data across 10-K, earnings release, and investor presentation.
3. **Management Claim Verification**: Management claims require proxy statement (DEF 14A) verification.
4. **No Price Target Without Methodology**: Price targets must include method, assumptions (A-XXX), range, and horizon.
5. **Assumption Registry**: All assumptions registered with A-XXX codes and sensitivity ratings.
6. **Competitor Verification**: Verify competitors exist and compete before citing.
7. **Read-First, Filing-Only**: Base analysis on SEC filings, not general knowledge.
8. **Gap Over Fabricate**: Missing data = GAP tag, never fabrication.

### HFRT Rule 6: Gate Enforcement
Three mandatory gates must pass:
- **Research Sufficiency Gate** (Phase 2→3): Templates 00-06 complete, all claims cited, assumptions registered.
- **DD Sufficiency Gate** (Phase 3→4): Templates 07-09 complete, risk register populated, governance reviewed.
- **Thesis Coherence Gate** (Phase 5, before final memo): Bull/bear balanced, catalyst identified, downside quantified.

### HFRT Rule 7: Dialectic Protocol
@Bull_Analyst and @Bear_Analyst operate in complete isolation:
- Both launched in parallel but separate contexts
- Neither sees the other's review
- Both submit to @HFRT_Commander who passes to @Thesis_Synthesizer
- Only @Thesis_Synthesizer sees both reviews together

### HFRT Rule 8: Research Invariants Enforcement
8 invariants must hold for all research output:
1. No thesis without quantified downside
2. No position recommendation without identified catalyst
3. All data verifiable (citations required)
4. Management claims require proxy verification
5. Valuation requires multiple methods
6. Quality of earnings review mandatory
7. Governance review mandatory
8. Liquidity check required

### HFRT Rule 9: User Authority
All HFRT decisions are recommendations—the user always has final authority:
- Users can override agent recommendations
- Users can request additional research
- Users can accept research with documented gaps
- Users cannot override anti-hallucination violations (fabrication never acceptable)

### HFRT Rule 10: File Coordination
HFRT agents coordinate through the project folder structure:
```
[project]/
  research_template/  -- 15 working research documents (00-14)
  .hfrt/             -- PROGRESS.md, BULL_REVIEW.md, BEAR_REVIEW.md
  frameworks/        -- Analysis frameworks (read-only reference)
```

### HFRT Rule 11: Resource Governance
To prevent resource exhaustion:
- **Concurrency:** Max 1 concurrent agent (except Phase 4 Bull+Bear)
- **Checkpoints:** Logged to .hfrt/PROGRESS.md after each agent completes
- **Timeout:** If agent runs >15 minutes without output, Commander reports to user
- **Emergency Halt:** User can say "HALT HFRT" at any time

### HFRT Rule 12: Recovery Protocol
**EMERGENCY HALT:** User can say "HALT HFRT" or "STOP" at any time.

**Recovery from Interruption:**
1. Read .hfrt/PROGRESS.md to find last checkpoint
2. Invoke @HFRT_Commander with: "Resume research on [TICKER] from Phase [X]"
3. Commander reads PROGRESS.md and resumes from last valid state

## Reference
HFRT documentation: See `HEDGE_FUND_RESEARCH_TEAM_GUIDE.md` in the Hedge Fund Research Team folder.

<!-- ============================================================ -->
<!-- END OF TOPIC: EQUITY RESEARCH                                -->
<!-- ============================================================ -->
```

---

## Installation Instructions

1. Open your global CLAUDE.md:
   ```
   C:\Users\[username]\.claude\CLAUDE.md
   ```

2. Add the section above after the existing framework sections (SDE, SST, BPT)

3. Save the file

4. Verify by starting a new Claude Code session and running:
   ```
   @HFRT_Commander, research AAPL
   ```

---

## Verification Checklist

After adding to CLAUDE.md, verify:

- [ ] HFRT Rule 1-12 are present
- [ ] Domain separation from SDE/SST/BPT is clear
- [ ] Anti-hallucination protocols are listed
- [ ] Gate requirements are specified
- [ ] Recovery protocol is documented
