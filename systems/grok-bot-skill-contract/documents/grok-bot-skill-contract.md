<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Write a Grok Bot Skill Twice

**Project Link:** [View Project](https://nextwork.ai/projects/5fe40fdc-8c43-4883-ae3a-9b95bdf98b87)

**Author:** Roy Piring: Cloud Platform Engineer | Build Master  
**Email:** rpiringhawaii@gmail.com

---

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/5fe40fdc-8c43-4883-ae3a-9b95bdf98b87_sfkw77np)

## Project Overview: Building an AI Curation Bot

### What this project set out to achieve

I built two versions of a newsletter curation skill for my Grok Bot and designed a controlled comparison between them. Version A used four fields as the baseline. Version B added explicit validation criteria and approval boundaries. The goal was to test whether those added instructions reduced the number of flawed items reaching the shortlist.

The planted failures represented two editorial risks: content without a primary source and content outside the allowed date window. I planned to run the same candidate batch through both versions and compare their results. This design separated the wording change from the test data. It also required me to report what the run produced, including the case where neither skill received enough input to generate a shortlist.

## Setting Up the Curation Bot

### Installing Grok Bot and configuring the curation assistant

I set up Weekly Curation A as the first Grok Bot skill using a four-field structure. This version established the baseline for how the Bot handled candidate items without explicit rules for missing primary sources, stale content, or borderline approvals.

Starting with the smaller skill mattered because Version B needed a clear point of comparison. If both versions contained the same validation rules, I could not isolate what the two added fields changed. The setup also separated skill configuration from test execution. Saving Weekly Curation A made the instructions available to the Bot, but it did not prove the Bot could filter the candidate batch. That required a separate run where the numbered items were sent, scored, and returned as a shortlist.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/5fe40fdc-8c43-4883-ae3a-9b95bdf98b87_ugsehc0p)

## Designing the Test Batch

### Assembling a 10-item list with planted flaws

I created a batch of 10 candidate items and planted two failures with different causes. One item lacked a primary source, while the other fell outside the 7-day date window. Each item violated one criterion, so its expected result was documented before either skill processed the batch.

This structure was meant to test whether the Bot could reject known failures without discarding compliant items. It also created an objective score because I did not need to decide afterward whether an article felt weak. candidate-batch.md recorded each item and its expected outcome.

The batch later expanded from 10 to 12 items when I added two borderline cases. Those additions tested how the wording handled ambiguous source and date boundaries rather than only clear violations.

### Flaws planted to test the Bot's filtering ability

I saved "three lights" as a standing three-line preference before documenting the planted cases. Item A was the Analytics Insight Slack Code article dated Aug 21. It failed the source requirement because it did not link to Slack's own post and provided no primary source.

Item B was Cursor's "Google Workspace Plugins" article dated Aug 3. It failed the freshness requirement because it was 19 days old and outside the 7-day window. The details were recorded in candidate-batch.md.

Each item violated exactly one rule. That separation mattered because a rejection could be traced to one known cause. If an item failed both source and date checks, the result would not show which validation rule had affected the Bot's decision.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/5fe40fdc-8c43-4883-ae3a-9b95bdf98b87_3qobn58n)

## Running Version A: Four-Field Curation Skill

### Writing and executing the first prompt version

I saved Weekly Curation A as the four-field baseline skill and prepared to run it against the 10-item candidate batch. Version A covered the basic curation task but did not contain explicit instructions for detecting missing primary sources, enforcing the 7-day window, or asking before including borderline items.

That omission was intentional. The baseline needed fewer controls so I could see whether Version B's source, date, and approval rules changed the shortlist. However, saving the skill alone did not complete the test. The numbered candidate batch also needed to be sent in a form the Bot could score.

Because that input step did not happen, Version A never produced the planned shortlist. Its result documented an incomplete run rather than evidence about filtering accuracy.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/5fe40fdc-8c43-4883-ae3a-9b95bdf98b87_2z5eu8mb)

### Analysing how many planted items slipped through

The recorded result was 0 of 2 planted items making it through, but that count did not represent successful filtering. Version A never received a numbered candidate batch and therefore produced no shortlist. With no evaluated list, neither planted item had an opportunity to appear.

This distinction prevents an empty output from being treated as a perfect score. A filter that rejects both flawed items should also process the remaining candidates and return a valid shortlist. Version A did neither because the required input was missing.

The run established an execution gap rather than a quality result. It showed that the skill and candidate batch must be connected before the test can measure editorial behavior. Until that happens, 0 of 2 means no exposure, not two correct rejections.

## Running Version B: Six-Field Curation Skill

### Expanding the prompt with two additional fields

I expanded the baseline into a six-field skill by adding validation criteria and approval boundaries. The validation rules required source and date checks, while the approval boundary required the Bot to ask before including borderline content or sending anything externally.

These additions made the expected behavior more explicit. Version B was supposed to reject items without the required source, exclude items outside the date window, and stop for confirmation when a decision crossed the stated boundary. The comparison still depended on sending the same candidate batch to both versions.

Version B did not receive a scored batch during this run, so the added fields were not exercised against the planted failures. The skill design changed, but the observed output did not provide evidence that those instructions changed the filtering result.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/5fe40fdc-8c43-4883-ae3a-9b95bdf98b87_r17lrlho)

### Comparing Version B's filtering performance to Version A

Version A recorded 0 planted items in a shortlist, and Version B recorded the same count. Those numbers look equal, but they do not represent two successful filtering runs. Neither version produced a shortlist because no numbered candidate batch was sent for scoring.

As a result, Version B's added source and date rules had no chance to affect the outcome. The comparison cannot show that the six-field skill filtered better, worse, or the same as the four-field skill. It only shows that both runs stopped before candidate evaluation.

The honest conclusion is that the experiment remained incomplete at this stage. A valid comparison requires both skills to receive the same numbered list, return shortlists, and expose whether Item A and Item B were rejected for the intended reasons.

## Scoring the Results: Pass or Fail

### Evaluating the Bot across three quality checks

I defined three checks for comparing the two skills: whether the planted failures were excluded, whether compliant items remained available for selection, and whether the added fields caused excessive filtering. Each check needed a pass or fail result based on an actual shortlist.

The missing batch prevented those checks from measuring filtering accuracy. An empty result cannot prove that flawed items were correctly rejected, and it cannot show whether valid items were preserved. It also provides no evidence about over-filtering because the Bot evaluated nothing.

The scoring step therefore exposed a problem with the test execution rather than the skill rules. The criteria were defined, but the required output was absent. The next valid run must connect both skill versions to the same candidate batch before any pass or fail result can support a decision.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/5fe40fdc-8c43-4883-ae3a-9b95bdf98b87_sfkw77np)

### Deciding whether the extra fields are worth keeping

The two added fields introduced source and date gates, plus a requirement to ask before including borderline material or sending content externally. These controls made the editorial boundaries clearer than the four-field baseline.

This run did not prove that the fields changed the shortlist because no batch was scored. The equal A 0 and B 0 counts came from missing outputs, not equal filtering behavior. I therefore could not claim that Version B reduced the planted failures during this comparison.

I chose to keep the fields because they state the checks the Bot should apply when a real candidate list arrives. That decision was based on the intended control they add, not on the incomplete score. Their effect still needs to be measured through a full side-by-side run.

## Going Further: Scaling to 12 Items

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/5fe40fdc-8c43-4883-ae3a-9b95bdf98b87_5eno8se8)

### Refining validation criteria for borderline items

I added two borderline cases to test how the validation wording handled ambiguity. Item C clarified that "primary source" meant independent reporting rather than a vendor press release. Under that rule, the Cursor changelog remained outside the shortlist.

Item D tested the date boundary. I defined the 7-day window as inclusive of both boundary dates, so the Aug 16 Copilot article remained eligible. This prevented the Bot from treating the first or last date as automatically outside the window.

The revised wording produced the intended decisions when I reran the cases. I made no second edit because the source rule excluded Item C and the inclusive date rule retained Item D. These cases expanded the test batch from 10 to 12 items and tested judgment at the boundary, not only obvious failures.

## Reflections and Key Takeaways

### Tools and concepts mastered

I used the Grok Bot platform to create, save, and compare two curation skills. I also built a custom 12-item batch containing planted failures and borderline cases. The batch gave the skills controlled inputs with expected outcomes instead of relying on an unstructured set of newsletter links.

The main concept was adding explicit validation fields to an AI skill. Source rules, date rules, and approval boundaries turn broad curation instructions into decisions that can be checked. I also learned why planted failures must be processed before their absence carries meaning.

The borderline cases showed that precise wording matters at category edges. "Primary source" and an inclusive 7-day window needed clear definitions so the Bot could apply them consistently without filtering valid content by accident.

### Time invested and challenges overcome

This build took approximately 40 minutes. The hardest part was writing validation criteria that handled borderline cases without excluding high-quality content that met the intended rules. The source definition and date-window boundaries required enough detail to produce repeatable decisions.

### Looking ahead to the next skill

The run also exposed an execution lesson. A saved skill and prepared batch do not create a comparison until the Bot receives the numbered candidates and returns a shortlist. The A 0 and B 0 counts could not prove filtering accuracy because neither version scored the list.

I completed the build to understand how validation fields shape Grok Bot curation. My next step is to run both skills against the same full batch, compare their shortlists, and then extend the selected skill to draft newsletter summaries from the approved items.

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/5fe40fdc-8c43-4883-ae3a-9b95bdf98b87)*
