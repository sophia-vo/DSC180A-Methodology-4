---
layout: default
title: DSC 180A Methodology Assignment 4
---

# Sophia Vo

**Email:** [sovo@ucsd.edu](mailto:sovo@ucsd.edu)

**Section:** D23 (Intuit) — Red-Teaming the Swarm: Autonomous Auditing for Financial LLMs

**Mentors:** Dhruvi Kothari and Victor Calderon

---

## Quarter 2 Project Brainstorming

**1. What is the most interesting topic covered in your domain this quarter?**

The most interesting topic was that an agent can reach the right answer for the wrong reason, and that outcome-based grading cannot tell the difference. In τ-bench, an agent passes a task if the company's records end up correct, but the same agent often fails when it has to succeed on every one of several repeated runs (pass^k). DFAH-Bench found something similar for financial agents: runs usually agreed on the final decision but took different steps to get there. Together with work showing that models give up a correct answer when a user pushes back, this changed how I think about evaluation. A correct final answer is not enough; we also need to check that the decision rests on the right evidence, and that it stays correct when the facts change. That idea became the core of our Quarter 1 benchmark.

**2. Describe a potential investigation you would like to pursue for your Quarter 2 Project.**

I would like to test whether outdated facts spread when a financial review is split across several agents. In our Quarter 1 benchmark, one agent reviews a credit renewal under a written policy while one figure in the report changes partway through the conversation. In Quarter 2, we could split the same review into roles, such as one agent that extracts figures from the report, one that checks them against the policy, and one that makes the decision, with each agent passing a written memo to the next. Because our families already record the correct facts and the correct decision after every change, we could trace exactly where a corrected figure is lost: whether the extractor never updates it, the memo carries the old value forward, or the decision maker ignores it. A related investigation, if the multi-agent work moves faster or slower than expected, is to add the third kind of change our research question names but Quarter 1 leaves out: changes to approvals, such as a manager withdrawing an approval after the agent has already acted on it. A second option is to test whether a simple fix, such as giving the agent a tool that keeps a running record of each fact and the document that supports it, reduces invalid approvals and unnecessary blocks.

**3. What is a potential change you'd make to the approach taken in your current Quarter 1 Project?**

I would widen the set of metrics the benchmark tests. Right now, every family checks one growth rate against one threshold, because most ConvFinQA conversations end in a growth rate (54.3% of the conversations that break a single question into steps) and growth rates are the easiest metric to edit safely. Under our current rules, however, only 7 growth-rate candidates from 5 companies pass every check, and the held-out set has none, so the benchmark risks being both small and narrow. I would add a template for ratio metrics, such as margins or coverage ratios, and write the renewal policy so that it combines two or three metrics, as public bank credit policies do. This would make the decisions closer to a real credit review and would give us more families from more companies. It would also let us include more cases where the correct decision is to renew, since three of our four current families end in a decline.

**4. What other techniques would you be interested in using in your project?**

First, causal replay: when an agent fails a version, we could rerun the conversation from a single turn with that turn changed, to find the exact step that caused the wrong decision rather than only scoring the final answer. Second, dialogue state tracking, so we can compare the agent's record of the facts with the correct record after every turn instead of only at the end. Third, more careful statistics for comparing versions. Each pair of versions shares the same table, so a paired test such as McNemar's test fits better than comparing overall accuracy, and because several families share a company, a mixed-effects model with a company-level effect would keep one company from driving the results. Finally, I would like to try an LLM judge for the parts of a response that code cannot check, such as whether the agent's explanation cites the right document, and measure how often that judge agrees with the answers our code computes.