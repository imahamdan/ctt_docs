# Vmin Search Direction and Plan

## Direction
1. Create a standalone VminSearch Code and services
2. Split all Features into a small testable & reusable modules 
3. develop a bridge per PRIME Version supporting PRIME12&PRIME13
4. Replace search ranges by dynamic and smarter rangeHandler
5. Fork SCRDB and reuse dedicated PRIME template


## Refactoring Status & Plan

| # | Description                                                    | Status   |
|---|----------------------------------------------------------------|----------|
| 1 | Learn LNL VminSearch template and document all interactions    | 80% Done |
| 2 | Learn PTL gaps and document it                                 | Done     |
| 3 | Define configuration files required for VminSearch and AutoGen | 90% Done |
| 4 | Design VminSearch Director and basic services                  | Wip      |
| 5 | Review the design with Prime team & DDG team                   | Defined  |
| 6 | KickStart Code Development                                     | Defined  |
| 7 | Develop PRIME12 Bridge and Deploy in LNL Engineering Program   | Defined  |
| 8 | Deploy newly developed VminSearch in PTL-P Engineering Program | Defined  |
| 8 | Deploy newly developed VminSearch in PTL-P Engineering Program | Defined  |

## PLAN Gant
```plantuml
@startgantt
saturday are closed
friday are closed

Project starts 2024-07-20
-- Learning and Documentation (Kochav & Ahmad)--
[Lean LNL VminSearch] as [LNL_S] requires 10 days
[Learn PTL VminSearch] as [PTL_S] requires 2 days
[Define System InputFiles] as [IN_S] requires 3 days

[Review Design] as [REVIEW_S] requires 2 days
[Review Design] as [REVIEW_S] starts at 2024-08-20

-- Coding Kick Start (Kochav & Ahmad)--
[Design &  CODING] as [CODING] requires 25 days
[Design &  CODING] starts  2024-08-2

-- On Tester Validation (Kochav) --
[LNL_SI_VAL] requires 5 days
[PTL_SI_VAL] requires 5 days
[LNL_SI_VAL] starts 1 working days after [CODING]'s end and requires 5 days
[APPLY_LEARNINGS] starts 1 working days after [LNL_SI_VAL]'s end and requires 3 days
[PTL_SI_VAL] starts 1 working days after [APPLY_LEARNINGS]'s end and requires 5 days


-- Checkers Development (Alex)) --
[SMART-TP CHECKERS] requires 20 days
[SMART-TP CHECKERS] starts  2024-08-15

-- AutoGen (Ahmad, Doron & Naphtali)--
[OTPL Formatter] requires 7 days
[AUTOGEN_MVP] requires 5 days
[AUTOGEN_PHASE2] requires 15 days
[OTPL Formatter] starts  2024-08-20
[AUTOGEN_MVP] starts  2024-08-18
[AUTOGEN_PHASE2] starts 1 working days after [AUTOGEN_MVP]'s end and requires 10 days
[AUTOGEN_TP_VAL] starts 1 working days after [AUTOGEN_PHASE2]'s end and requires 5 days
@endgantt

```
# Help Needed
1. TORCH Support for integrating AUTO-GEN natively
2. TRACE Support for new MOW & References usage
3. TORCH Support for an integrated UI handling configuration files
4. Prime Support and required new features development & Template ownership transformation
4. PTL Validation Support once tempalte is Ready