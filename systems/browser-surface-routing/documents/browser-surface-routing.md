<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Which Browser Did Claude Just Open?

**Project Link:** [View Project](https://nextwork.ai/projects/76293993-da49-452f-a3c2-ed426fc61815)

**Author:** Roy Piring: Cloud Platform Engineer | Build Master  
**Email:** rpiringhawaii@gmail.com

---

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/76293993-da49-452f-a3c2-ed426fc61815_l415q5i6)

## Project Overview

### Goals and approach

I tested how Claude’s Cowork feature handled web navigation across its available browser settings. The build focused on one visible question: whether a web task opened in Cowork’s built-in Browser pane or in my local Chrome browser through the Claude in Chrome extension.

I first recorded the available browser surfaces and made a prediction before opening a public webpage. I then observed where the page appeared, changed the Preferred browser setting, repeated the task, and compared both runs.

The method separated settings labels from observed behavior. A selected option did not prove which surface handled the task. The active pane, Chrome tabs, connector state, and tool activity provided the evidence. I also recorded missing details as unknown instead of filling them from assumption, keeping confirmed findings separate from predictions and user-reported observations.

## Recording the Rollout: Did the Built-In Browser Arrive?

### What this step set out to find

I configured the local environment and checked whether the Cowork browser surfaces were available before running any navigation test. This established the baseline needed to identify which browser opened later and prevented missing setup from being mistaken for routing behavior.

I confirmed that the Chrome extension was installed and connected, the connector was on, and the built-in Browser pane was available on the account. Both paths therefore existed before I started the web task.

I also kept the workspace isolated and recorded the findings in notes/browser-surfaces-record.md. That file separated observed facts from fields I could not verify. The plan tier, exact Preferred browser wording, and selected option remained blank because I did not have the settings screenshot. This baseline gave the later test a known starting point without claiming more than the available evidence showed.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/76293993-da49-452f-a3c2-ed426fc61815_8dje5o16)

### Dated finding with plan tier

I dated the finding 2026-08-27 in notes/browser-surfaces-record.md. My pre-check prediction was “present” because the mcp__Claude_Browser__* tools loaded natively instead of appearing as deferred tools. I treated that behavior as a prediction signal, then checked it against visible tool evidence.

The Chrome extension was installed and connected through device a405d1da…, named “Browser 1,” on a local Windows system. The connector was on, and the built-in Browser pane was available on the account.

Three fields remained unknown: the plan tier, whether “Preferred browser” appeared literally under Cowork, and which option was selected. I never received the settings screenshot, so I left those fields blank. The dated record therefore confirmed the available surfaces while keeping account and setting details outside the verified result.

## Watching Which Browser Actually Opens

### Baseline observation method

I started a web task from Claude Desktop and watched both possible browser surfaces. The test checked whether the requested page appeared inside Cowork’s built-in Browser pane or as a new tab in my local Chrome instance.

Before running it, I predicted that the built-in pane would handle the request. This session declared that surface as its default, while local Chrome was reserved for tasks requiring my signed-in browser profile. The selected Wikipedia page did not require a login.

I then checked the Cowork pane, my open Chrome windows, and the Claude in Chrome connection. This made the result depend on observable browser activity rather than Claude’s description of what it opened. The baseline also created a condition for the later reversal test. To prove that a setting changed routing, the second run needed to begin from a different selected surface and create a different observed result.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/76293993-da49-452f-a3c2-ed426fc61815_l415q5i6)

### Prediction vs. observation

The Cowork built-in Browser pane handled the task. It opened inside Claude’s side panel as a tab seed through mcp__Claude_Browser__preview_start. No page appeared in my real Chrome browser, and the connected Claude in Chrome extension remained idle during the run.

This matched my prediction and the reason recorded before the test. The target was a public Wikipedia page that did not require cookies or a logged-in Chrome profile, so I expected the session’s declared built-in surface to handle it.

One evidence limit remained. This prediction was created during the current session, not during the “Step 0” assumed by the exercise. No earlier Step 0 prediction existed in the available record. I therefore logged the comparison as a match for this session while keeping it separate from the missing earlier prediction the exercise expected.

## Testing the Preferred Browser Setting in Both Directions

### Why the reversal matters

I tested the Preferred browser setting because a visible option does not prove that it controls browser routing. A meaningful test required selecting one surface, running the same task, switching to the other surface, and repeating the request under otherwise matching conditions.

The expected evidence was a change in where the page opened. If the first run created a local Chrome tab and the second opened the Cowork Browser pane, the reversal would support the claim that the setting controlled the path.

A repeated result would require more investigation. It could mean the setting was ignored, the active Cowork session had not loaded the change, or both runs used the same setting. This is why the direction of the switch and the session boundary mattered. Without different starting conditions, a before-and-after comparison could look complete while testing no routing change.

### Switch, repeat, and reversal results

The repeated task opened the built-in Browser pane again as a tab seed, and no Chrome tab appeared. The article remained “Lord Mountbatten.” However, the comparison was void because the Step 2 baseline had already used the built-in pane. Switching to the built-in option did not create a different condition.

If the earlier setting had truly been Chrome, the Step 2 result would suggest that the selected preference was not honored. A second explanation was that the running Cowork session did not load the new setting. Testing that possibility required selecting Chrome and starting a fresh Cowork task before repeating the same request.

I recorded this caveat in notes/browser-surfaces-record.md. The two observed runs confirmed the same built-in surface, but they did not prove whether the Preferred browser label controlled routing. A valid reversal still required Chrome as the selected path and a new session.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/76293993-da49-452f-a3c2-ed426fc61815_p1bnufpg)

## Reading the Login-Import Offer Without Accepting It

### What this step examined

I examined the login-import offer shown by the built-in Browser pane without accepting the import. The goal was to identify which source browsers the dialog named and compare that observed list with the vendor’s published documentation.

This step focused on the offer itself rather than moving personal browser data. Reaching the dialog established that the built-in browser could present an import path, while leaving it unaccepted avoided changing the browser state during the observation.

I recorded the source browser I knew was used and kept unobserved wording blank. This mattered because the final comparison depended on the exact names displayed by the dialog. Remembering likely browser options or filling gaps from general knowledge would weaken the record. The evidence needed to come from the visible offer or a user-reported observation clearly marked with its source.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/76293993-da49-452f-a3c2-ed426fc61815_hpupqaq7)

### Platform findings vs. vendor's published list

In Observation 4, I reached the login-import offer inside the Cowork side panel’s built-in browser and used Firefox as the source browser. I recorded that result in notes/browser-surfaces-record.md as user-reported and attributed it to the user rather than presenting it as something I independently verified.

The full list of source browsers named by the dialog was not recorded. I also did not preserve the dialog’s exact wording. Those fields remained blank because Firefox was the only source browser confirmed in the available observation.

The comparison with the vendor’s published list therefore remained open. Completing it required the names displayed in the dialog and the corresponding vendor documentation. Until both sources were available, I could confirm only that Firefox was used in this run. I did not infer the remaining browser names or claim that the observed and published lists matched.

## Secret Mission: Applying the Method to Another Tool

### Testing whether a setting between two paths is real

I applied the same method to a setting that claimed to choose between two paths. The test required a prediction, one controlled run, a setting change, a fresh session when needed, and a repeated task. The final judgment had to come from which path executed, not from the selected label alone.

For the Claude browser test, the built-in pane ran with user agent Claude/1.37937.3 … MSIX and had no imported cookies. That behavior matched the design of an isolated browser surface. I kept broader expectations out of the observations log because they were hypothetical rather than verified.

The Chrome leg remained the missing test. It was the only run that could expose an ignored or session-scoped preference. I would need to select Chrome, start a fresh Cowork task, repeat the same request, and inspect both surfaces. Until then, the record showed repeated built-in behavior but not a proven two-way switch.

## Reflections and Takeaways

### Key tools and concepts

I used Claude Desktop, the Claude in Chrome extension, and Cowork’s built-in Browser pane. Together, these surfaces let me compare a browser contained inside Claude with a connected local Chrome instance.

The main method was predict, toggle, observe, and reverse. I recorded the expected path before running the task, changed the relevant setting, repeated the same request, and checked the browser surfaces directly. This separated interface labels from execution evidence.

I also learned to preserve unknown fields. The missing plan tier, settings screenshot, selected option, complete import list, and vendor comparison remained blank rather than becoming assumptions. The isolated workspace and dated notes kept verified findings, user-reported observations, and hypothetical behavior separate. That structure made the rollout record useful even though the reversal test was incomplete.

### Time and challenges

This build took approximately 25 minutes. The hardest part was finding a site that triggered the cookie-import offer inside the built-in browser. The initial Wikipedia page opened successfully, but it did not display the import path needed for that observation.

The browser-routing test also exposed a design challenge. Both recorded runs used the built-in pane, so the before-and-after comparison did not contain two different conditions. The repeated result could not prove that the Preferred browser setting controlled either path.

The missing settings screenshot added another limit. Without it, I could not confirm the plan tier, exact label, or selected browser. I preserved those gaps in the notes instead of reconstructing them later. The remaining work was a fresh Cowork task with Chrome selected, followed by direct inspection of both browser surfaces.

### What I learned and what's next

I completed this build to examine how Claude’s Cowork feature handled browser preferences and login-import behavior during its current rollout. The observed web task opened in the built-in Browser pane, while the connected local Chrome browser remained idle.

The exercise showed that a repeated outcome does not prove a setting works when both runs begin from the same condition. The attempted reversal stayed inconclusive because the baseline already used the built-in pane. It also showed why user-reported findings, tool evidence, predictions, and unknown fields should remain distinct.

My next step is to test Chrome as the selected preference in a fresh Cowork task and repeat the same public-page request. I also want to apply this prediction-testing-reversal method to other software settings that claim to choose between two execution paths or integrations.

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/76293993-da49-452f-a3c2-ed426fc61815)*
