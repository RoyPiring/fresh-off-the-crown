<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Measure What Concise Drops

**Project Link:** [View Project](https://nextwork.ai/projects/1230eca0-ef03-4cf2-8b80-ee56c4a93c6c)

**Author:** Roy Piring: Cloud Platform Engineer | Build Master  
**Email:** rpiringhawaii@gmail.com

---

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/1230eca0-ef03-4cf2-8b80-ee56c4a93c6c_duvz8cyn)

## Understanding the Experiment

### Defining the measurement and what would change the outcome

I measured whether Claude Code's Concise output style omitted critical information compared with Default. I used the same six prompts for both styles, counted the lines in each answer, and scored each response against required items defined before testing. This separated answer length from factual coverage.

My decision rule was set in advance. I would question the value of Concise if shorter responses repeatedly failed to include mandatory scorecard items that Default retained. If both styles preserved the same required facts, the line reduction would count as useful compression within this test.

The method did not assume that shorter or longer was better. It required the output to show whether reduced length was connected to information loss.

## Designing the Test: Six Prompts with Pre-Committed Required Items

### Why locking required items before running anything matters

I created a sample codebase, six prompts, and a scorecard before running either output style. Each prompt had three required items, creating 18 fixed facts for the comparison. Defining them first prevented me from changing the grading rules after seeing which answer I preferred.

The required items also turned a broad question about quality into specific checks. A response either stated the needed fact or it did not. Line count then measured how much text each style used to deliver those facts.

This setup reduced confirmation bias because both styles faced the same prompts and scoring criteria. It also made the limits of the test visible. The required items focused on concrete code facts, so the scorecard did not test every kind of information that brevity could remove.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/1230eca0-ef03-4cf2-8b80-ee56c4a93c6c_5tl8iynl)

### Reasoning behind one prompt's required items

For one prompt, the first required item was that HTTP 4xx errors are not transient. Retrying a 404 or 401 three times wastes 6 seconds before reaching the same failure. This was the direct answer to the question.

The second item was the bare except Exception at api_client.py:16. It cannot distinguish a timeout from a KeyError on data["unit_price"], so the code retries a programming bug as if it were a network failure.

The third item was the consequence: after 3 failures, the function silently continues. The SKU disappears from out without an exception or log. The caller therefore cannot distinguish a missing price from a real result. Together, the three items covered the answer, cause, and operational effect.

## Running the Baseline: Default Output Style

### What the Default run establishes

I ran all six prompts with the Default output style to establish the baseline for answer length and required-item coverage. Each answer was measured by line count and scored against the three items assigned to that prompt before the run.

The baseline served as the control for the later Concise comparison. Without it, a shorter response could appear cleaner without showing what had been removed. The scorecard checked whether the answer preserved the facts needed to address the question, while the line count measured how much text it used.

I did not treat Default as better because it produced more lines. Its role was to show the normal amount of explanation and provide a fixed result against which the shorter responses could be judged.

### Checking a required item against an actual answer

I checked the required consequence against the paragraph beginning "Worse:". The answer stated that the loop ended after 3 attempts, moved to the next SKU, and produced no exception or log. That language satisfied the item directly rather than leaving the outcome implied.

The response also explained the caller's problem. The failed SKU was absent from out, so the caller could not distinguish a missing price from one that had never been requested.

I used a strict grading rule. Saying only that the code "gives up after 3 tries" would have scored the item absent. The answer needed to state that the failure was silent and identify the missing exception, missing log, or effect on the output. This kept the score tied to the full required claim.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/1230eca0-ef03-4cf2-8b80-ee56c4a93c6c_598l7akb)

## Switching to Concise and Running the Same Six

### How the style switch was applied

I switched Claude Code to the Concise output style and reran the same six prompts. The prompts, sample code, and 18 required items remained unchanged so the output approach was the intended comparison variable.

For each response, I recorded the line count and checked whether all three required facts were stated. This made the comparison about measurable retention rather than tone or personal preference.

A later review exposed an important limit. The Concise answers came from the Default session written tersely rather than a loaded outputStyle: Concise prompt. The run therefore measured deliberate compression within that session, not a fully isolated test of Claude Code's configured Concise style. The results remain useful, but any conclusion must stay within that boundary.

## Reading the Scorecard: What the Numbers Say

### Comparing line counts and required-items totals

I compared the total lines and required-item hits across the six Default and six Concise responses. The scorecard tested whether the shorter answers removed repeated explanation or sacrificed facts marked as necessary before the run.

Default used 37 lines, while Concise used 22. That was a 41% reduction. Both styles retained all 18 required items, and every prompt scored 3/3 in both columns.

Within this batch, the shorter responses preserved every concrete code fact included in the scorecard. The result does not prove that Concise retains every kind of critical information. None of the required items tested a caveat, refusal, safety boundary, or uncertainty statement. The measurement supports the tested facts only, not a general rule about all concise answers.

### Which prompts and items differed between styles

The required-item totals were 18 vs 18. Every one of the six prompts scored 3/3 under both styles, so Concise dropped none of the pre-committed facts. The 41% line reduction came from preamble, repeated context, and secondary advice.

The perfect scores also exposed a weakness in the scorecard. Because both styles retained everything, the required-item metric could not separate their behavior. A comparison where nothing fails says more about the difficulty and scope of the prompts than the general safety of either style.

All 18 items were concrete, locatable code facts. The batch did not include a required caveat or refusal, which are more likely to disappear during compression. Those cases are necessary before the scorecard can test whether brevity removes a load-bearing limit.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/1230eca0-ef03-4cf2-8b80-ee56c4a93c6c_pdey5lt1)

### Data-backed conclusion on Concise vs Default

Across these six prompts, the shorter answers used 41% fewer lines, falling from 37 to 22, while both styles retained 18/18 required items. The removed text consisted of preamble, repeated context, and secondary advice rather than a scored fact.

The scorecard does not support a broad claim about Concise. The shorter responses were written tersely inside the Default session rather than generated with a loaded outputStyle: Concise prompt. Half of the rows were also graded against a report.py reconstructed from the answer key.

The defensible claim is narrow: compressing answers built around concrete, locatable code facts caused no measured loss here. Whether compression removes a required caveat or refusal remains untested and should be examined next.

## Secret Mission: Writing a Custom Output Style

### What the custom style changed compared to Default and Concise

On prompts 1 and 4, the custom style used 10 lines compared with Default's 15. It was 33% shorter and retained all required items. The removed content matched the Concise cuts: preamble, repeated context, and closing summaries.

The custom response was 25% longer than Concise, which used 8 lines. Its two extra lines preserved HTTP 429 as a retryable 4xx and explained that timeout=30 at api_client.py:13 applies per attempt. A hanging server could therefore cost ~90s/SKU rather than ~7s. Neither detail was scored, but both could change the next action.

All three styles retained 6/6 required items on those prompts. The custom style added correct decision context rather than recovering a miss.

## Project Reflections

### Key tools and concepts from the project

I used Claude Code for command-line interaction, Markdown with YAML frontmatter to define custom output styles, and a structured scorecard to compare the responses. These tools connected prompt configuration with measured changes in answer length and retained facts.

The main method was defining required items before running the prompts. This reduced confirmation bias because the grading rules could not be revised to favor the response I preferred. I also kept the six prompts and scoring criteria fixed while changing the output approach.

The build showed that information density requires more than counting lines. A shorter answer can retain every scored fact while removing unscored context that changes the next decision. The custom style protected two such details.

### Time taken and challenges encountered

This build took approximately 30 minutes. The hardest part was defining the required items before running any prompts. Each item needed to represent information required to answer the question rather than a detail I preferred after reading the output.

The scoring language also needed to distinguish direct coverage from implication. For example, saying that retries stop after 3 attempts was not enough. The answer had to state that the function continued silently without an exception or log.

That preparation kept the scorecard consistent, but it also revealed the metric's limits. All 18 items were concrete code facts. The test therefore allowed both styles to score perfectly without showing whether brevity would remove a caveat, refusal, uncertainty statement, or safety boundary.

### Motivation and next learning goals

I completed this build to measure how output style affects information retention instead of relying on a subjective impression that one answer felt clearer or shorter. The results showed that the tested compression reduced line count without removing any of the 18 required code facts.

The exercise also showed why a perfect score does not settle the broader question. The configured Concise style was not fully isolated, part of the comparison used a reconstructed report.py, and no required item tested a caveat or refusal.

My next step is to design harder prompts where the required information includes limits, uncertainty, safety boundaries, and reasons not to act. I also want to refine custom system prompts for specific Claude Code workflows.

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/1230eca0-ef03-4cf2-8b80-ee56c4a93c6c)*
