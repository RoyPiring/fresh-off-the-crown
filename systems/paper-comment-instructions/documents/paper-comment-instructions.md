<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Probe Paper's Comments with an Agent

**Project Link:** [View Project](https://nextwork.ai/projects/88d99a2b-8513-473a-ad6c-fc125e3b5e09)

**Author:** Roy Piring: Cloud Platform Engineer | Build Master  
**Email:** rpiringhawaii@gmail.com

---

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/88d99a2b-8513-473a-ad6c-fc125e3b5e09_pb6vv8m0)

## Connecting Paper and Cursor via MCP

### Setting up the tools

I connected Paper Desktop and Cursor through MCP so an AI agent could inspect a Paper file and act on comments attached to canvas elements. The build tested whether comments could function as instructions rather than notes intended only for human review. I needed the connection to support both reading the file and changing element properties.

I also created a controlled canvas before running the agent. The canvas contained elements with known fill, text, and padding values. These values provided the baseline needed to determine whether a later difference came from the agent.

The connection itself was only the delivery path. The result still had to be judged from the saved Paper artifact because an agent's chat response could claim a change that the file did not contain.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/88d99a2b-8513-473a-ad6c-fc125e3b5e09_krpx33cp)

## Defining the Experiment

### What this project tests and how the result will be judged

I tested whether Paper comments worked as actionable instructions when an AI agent accessed the file through MCP. The result was judged by changes to three recorded canvas properties, not by the agent's description of what it believed it had done.

The baseline contained a rectangle with fill #D1D5DB, the text "Draft Heading," and a frame with 24px padding. Each element received a comment under a different condition so I could compare an unresolved instruction, an unresolved observation, and a resolved instruction.

Before running the agent, I predicted which values would change. Afterward, I inspected the Paper file and compared its saved state with the baseline. This separated reported behavior from observable artifact behavior.

## Building Three Independent Elements and Recording the Baseline

### Why independent values matter

I created three independent elements on the Paper canvas: a gray rectangle, a text label, and a frame. Each element exposed one property for measurement. The rectangle tested fill, the label tested content, and the frame tested padding.

Keeping the values independent made each comment easier to score. A fill change could be connected to the rectangle comment without also affecting the text or spacing result. The baseline values were #D1D5DB, "Draft Heading," and 24px.

Recording those values before the agent ran established the starting evidence. Without a verified baseline, a matching final value could have existed before the comment was processed. The later secret mission also required resetting the same properties so the second run would not inherit changes from the first.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/88d99a2b-8513-473a-ad6c-fc125e3b5e09_czts63t5)

### Written prediction before the agent runs

I predicted that the rectangle fill would change because its unresolved comment gave a direct instruction and named the target color. The comment was open and supplied enough information for the agent to make a specific edit.

I predicted that the frame padding would remain 24px. Its comment was unresolved, but it only observed a spacing problem and did not provide a replacement value. My assumption was that recognizing a concern would not authorize the agent to select a number.

I predicted that the text would remain "Draft Heading." Its comment named the replacement string, but the comment had already been resolved. I treated the resolved state as a signal to skip the instruction even though the requested text was explicit.

## Placing Three Comments Under Three Conditions

### Unresolved instruction, unresolved observation, resolved instruction

I attached one comment to each canvas element under a different condition. The rectangle received an unresolved instruction that named a new fill. The frame received an unresolved observation about spacing without a replacement number. The text element received a resolved instruction containing an exact new string.

This design tested two variables at once: how specific the wording was and whether the comment remained open. The rectangle represented a direct, open command. The frame tested whether the agent would turn a vague observation into its own design decision. The text tested whether resolution blocked an otherwise clear instruction.

Because the three comments targeted separate properties, I could score their outcomes independently and then isolate any ambiguous result through another run.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/88d99a2b-8513-473a-ad6c-fc125e3b5e09_1307jbk5)

### What the agent reported versus what the file shows

For the rectangle, the agent found the open instruction and reported changing the fill to #3B82F6. This matched the expected response to a specific unresolved comment.

For the frame, it found the open observation even though the comment contained no number. The agent reported changing the padding from 24px to 40px. That result showed that it was willing to choose a value when the comment identified a problem but did not prescribe the correction.

For the text, the agent found the resolved instruction, reported that the heading was already "Final Heading," and left it unchanged. That statement did not establish whether the resolved comment had been obeyed. A final value matching the comment could have existed before the run, so the artifact needed an isolated reset test.

## Reading the File and Scoring the Prediction

### Judging results by artifact state, not agent chat

I inspected the Paper file after the agent finished and compared the rectangle fill, frame padding, and text content with the recorded baseline. The canvas state served as the evidence because chat output only described what the agent believed it found or changed.

This distinction mattered most for the text element. The agent said the heading was already "Final Heading" and made no edit. The final value matched the resolved instruction, but that match did not prove the agent applied it during the run.

Artifact inspection can confirm that a value changed between two verified states. It cannot assign cause when the starting state is uncertain or already matches the request. That limitation led to the secret mission, where the text was reset and the comment state became the isolated variable.

### The count that matters and what it means

The file showed values matching all 3 comments, producing a canvas score of 3 of 3. That count described the final artifact state. It did not prove that the agent had obeyed each type of comment during the main run.

The open instruction produced a specific fill change. The open observation also produced a padding change even though the agent had to choose 40px without a number in the comment. The resolved text instruction matched the final text, but the agent said the value was already present.

This exposed a limit in result scoring. A matching comment and matching property value do not establish who made the change or when it occurred. Causal proof required resetting the text and rerunning the same wording with only the resolved state changed.

## Secret Mission: Isolating the Resolved Instruction

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/88d99a2b-8513-473a-ad6c-fc125e3b5e09_63a1cpza)

### Whether resolving was the blocking factor

The isolated rerun showed that resolution was the blocking factor. I reset the text to "Draft Heading" and left the same instruction unresolved. The agent then changed the text to "Final Heading."

The rectangle fill and frame padding remained unchanged during this run. Only the property targeted by the open text instruction changed, which kept the comparison focused on the comment's resolution state.

The wording stayed the same while the state changed from resolved to unresolved. That produced a different result: the resolved instruction was skipped in the main run, while the open instruction was applied in the isolated run. Resetting the baseline removed the earlier ambiguity and tied the text change to the open comment condition.

## Reflections and Takeaways

### Key tools and concepts

I used Paper Desktop to create and inspect the canvas, Cursor to run the AI agent, and MCP to connect the editor with the Paper file. Together, these tools allowed the agent to read comments and modify design properties through one interaction path.

The main method was controlled comparison. I recorded baseline values, assigned separate comments to independent elements, and inspected the artifact after each run. This kept agent chat separate from the evidence stored in the file.

I also learned why isolation matters when testing agent behavior. The main run could not prove whether the resolved text comment had been applied because the final value already matched it. Resetting the text and changing only the resolution state produced a result that could be attributed to that variable.

### Time and challenges

This build took approximately 30 minutes. The hardest part was resetting the baseline before the secret mission. The rectangle, text, and frame needed to return to their intended starting values so changes from the first run would not contaminate the second.

The text case required the most care because a matching final value had created false confidence in the main score. Without resetting "Final Heading" to "Draft Heading," another matching result would still have failed to show whether the agent acted.

The isolated rerun corrected that weakness. The text changed only after the same instruction was left unresolved, while the fill and padding remained fixed. This made the resolution state the tested difference rather than one of several changing conditions.

I completed this build to understand how Paper comments behave as agent instructions when Paper and Cursor are connected through MCP. The tests showed that open comments can drive edits, including a vague spacing observation where the agent selected its own value.

The work also showed why artifact state alone can be misleading when the baseline is not controlled. A final value that matches a comment does not prove the agent produced it. The secret mission provided stronger evidence by resetting the text and changing only whether the comment was resolved.

My next step is to learn how to build and connect my own MCP servers. I want to apply the same baseline, isolation, and artifact-verification method to other design and productivity tools.

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/88d99a2b-8513-473a-ad6c-fc125e3b5e09)*
