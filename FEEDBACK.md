# Overnight review: Larkspur disruption-care agent

**To:** jaigandh_claude-base-camp-genious-group-repo  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-09-22 17:22

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Overnight, Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. agent.py is byte-identical to the shipped template, so no build work has landed in the repository yet.**

The static scan confirms TONE_ADDENDUM is still 0 characters, EXTRA_TOOLS is an empty list, and LOCAL_TOOLS has no executors. Every one of the six pencil marks (grep -n '✏' agent.py) sits at its starting value. Nothing in run_agent, build_tools, or tool_list has been touched by this team.

Run git diff against the template commit and paste the output to confirm whether any change exists.

**2. search_alternatives carries a 6-character description, just the word 'search', in build_tools().**

Every other tool schema runs from 71 to 445 characters, giving Claude fare rules, escalation triggers, and token requirements to reason from. search_alternatives gets one word, no mention of what counts as an alternative, no input beyond pnr, and no output shape. A model choosing which tool to call is choosing off this text, and no model size changes what text is there.

Run python3 run.py --show-tools and paste the full search_alternatives entry to confirm the description in production matches the 6 characters in the scan.

**3. No readout-trace.json and no evals/cases.json exist in this repository, so run_agent has never been observed to complete a loop.**

The scan states LAST COMMITTED WIRE RUN is none and EVAL CASES lists zero files. MAX_TOOL_CALLS is set to 8, but there is no trace showing whether any booking shape reaches stop_reason != 'tool_use' before hitting that cap or exits early. The while loop's exit behavior at turns == 8 is untested by anything in this repo.

Run python3 run.py K7PQ2M --trace and paste the turn count and final stop_reason.

**4. PITCH.md is unchanged from the template, so there is no stated cost, failure mode, or owner for this agent anywhere in the repository.**

The file list confirms PITCH.md is byte-identical to what shipped. No section of it names a dollar figure, a measured error rate, or a human queue that escalate_to_human hands off to. Priya's vendor question about model choice has nothing written against it to weigh, because nothing has been written at all.

Open PITCH.md and confirm whether any section header still contains placeholder text.

**5. check_policy's 445-character description asks Claude to track six fields including policy_row_id, with no eval case exercising the citation requirement.**

The schema requires cause_code, delay_minutes, and status be drawn from get_flight_status output rather than invented, and instructs 'cite it if you reference this decision again.' With zero entries in evals/cases.json, there is no example showing a policy_row_id actually get reused correctly across a later issue_voucher or confirm_rebooking call in the same run. That correctness depends on prompt structure and the loop's message history, not on which model reads it.

Run python3 verify.py 2.1 to check whether the policy citation behavior has a passing test case.

## Your four answers

The four lines under `## Priya asked` in your PITCH.md are still empty. They
are one line each and they are not a coding job: cost, what happens when it is
wrong, who runs it in June, and what you left out. Whoever on your side is not
editing agent.py is the right person to write them, and they are the four
things I will ask about first.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (226 lines)`
- `PITCH.md (unchanged template)`
- `TEAM.md (unchanged template)`

Reviewer: `claude-sonnet-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
