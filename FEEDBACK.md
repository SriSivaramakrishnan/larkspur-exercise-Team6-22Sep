# Overnight review: Larkspur disruption-care agent

**To:** SriSivaramakrishnan_larkspur-exercise-Team6-22Sep  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-09-22 17:21

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

**1. PITCH.md lists a token count that does not match the last committed trace in readout-trace.json.**

PITCH.md's Number line reads "Token count (22557 in / 950 out ... 15.8s)". readout-trace.json, the last committed wire run, shows 11663 in, 452 out, and a wall clock of 10.0s. Neither figure traces to a file in this repository.

Run python3 run.py --all --trace and compare the totals footer against the 22557/950/15.8s figure in PITCH.md.

**2. TONE_ADDENDUM is still 0 characters and EXTRA_TOOLS carries three new tool descriptions with no tone guidance behind any of them.**

The static scan confirms TONE_ADDENDUM at zero characters. The diff adds fare_rules, next_available_day and reopen_stats with long, carefully scoped descriptions, for example reopen_stats telling the model "this is for your own next action, not a figure to quote to the customer," but nothing constrains how the model should phrase what it says to a stranded passenger. A model swap changes none of this: the seam is empty regardless of which model reads it.

Fill TONE_ADDENDUM and run python3 verify.py 4.1 to see whether the intelligence gate reads it.

**3. search_alternatives in build_tools() still carries the description "search", six characters, unchanged by this pod's diff.**

The diff rewrote check_policy, hold_seat and confirm_rebooking's surrounding schema entries but left search_alternatives's description at the placeholder string "search". Every other tool this pod touched runs 200 to 1105 characters; this one tool that hands off option_id values to hold_seat and confirm_rebooking has no description at all for the model to read.

Run python3 run.py --show-tools and check whether search_alternatives still prints description "search".

**4. run_agent()'s loop caps at MAX_TOOL_CALLS = 8 and the last trace used 3 of them; nothing here shows what happens at the cap.**

The diff changed the loop to append response.content instead of text_of(response) and to return text_of(response) after the loop instead of a stale answer variable captured mid-loop. That is a real behavior fix for turns where the model's tool_use turn had no text. But the trace on file used tool calls: 3 against a cap of 8, so nothing here shows what a customer sees when the cap is hit mid-rebooking.

Run python3 run.py <PNR> --trace on a booking that needs more than 8 tool calls and paste the tail of the transcript.

**5. reopen_stats() and fare_rules() read from disk with no fallback if the corpus or excerpt file changes shape, and there are no eval cases to catch it.**

fare_rules() parses ### headings out of fare_rules_excerpt.md with a regex, and reopen_stats() computes reopen_rate as round(len(reopened) / len(matched), 2) straight off transcripts_sample.jsonl. There is no evals/cases.json in this repository, so nothing in the repo exercises what these two functions return when a section title doesn't match or a ticket_type has, as the tool description itself warns, a rate "drawn from one or two tickets."

Run python3 eval_harness.py after writing cases for fare_rules and reopen_stats against small-denominator inputs and paste the failures.

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

- `agent.py (385 lines)`
- `PITCH.md`
- `TEAM.md (unchanged template)`
- `readout-trace.json`
- `readout.html (evidence block)`

Reviewer: `claude-sonnet-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
