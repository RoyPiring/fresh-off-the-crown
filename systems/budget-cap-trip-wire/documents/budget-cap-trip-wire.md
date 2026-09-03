<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Test Claude Code Budget Controls

**Project Link:** [View Project](https://nextwork.ai/projects/492461ae-dceb-4e68-a5d2-065a101ebc8c)

**Author:** Roy Piring: Cloud Platform Engineer | Build Master  
**Email:** rpiringhawaii@gmail.com

---

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/492461ae-dceb-4e68-a5d2-065a101ebc8c_63pdbn6o)

## Measuring the Local Budget Stop

### Capturing a bounded print-mode run

I ran Claude Code in bounded print mode and captured the complete JSON result before extracting the fields used in the budget comparison. The evidence included the configured cap, reported total cost, result subtype, and values needed to calculate the displayed difference.

I preserved the raw output before interpreting it. This kept the report tied to the client response and prevented a copied summary from replacing the underlying result. I used decimal arithmetic for the subtraction and rounded only the displayed value.

The measurement tested whether the local client stopped after its computed cost crossed the supplied limit. It did not test authoritative billing or prove that an upstream service blocked spending before execution. The resulting evidence described how Claude Code 2.1.252 applied --max-budget-usd during this Windows print-mode run.

### Separating the cap, reported total, and calculated difference

The displayed difference came from subtracting two independently produced local values: reported_total 0.000973 minus cap 0.0001 equaled 0.000873. I kept both inputs as decimals and rounded the result to 6 places only for display, so no precision was lost at this magnitude.

The positive difference showed that the client completed and priced one Haiku turn before comparing the result with the cap and stopping. The control therefore behaved as a post-hoc trip wire rather than a gate that prevented any estimated spend beyond the limit.

The reported total was about 9.7 times the configured cap. That result showed that overshoot depended on the size of the first completed turn, not only on the cap value. The difference represented Claude Code’s local estimate and was not an invoice, charge record, or authoritative billing amount.

## Testing the Interactive Mode Boundary

### Checking whether the print-mode control applies interactively

I ran Claude Code without --print while keeping --max-budget-usd set to 0.0001. The purpose was to test whether the budget flag affected a non-print invocation or remained limited to the scope described by the client help.

I submitted the same CONTROL CHECK. target and watched for either a normal model response or a budget stop. The run halted immediately with an error and produced no model turn, showing that the flag was active in this non-print path.

The test environment did not include an attached TTY, so the result established non-print enforcement but not full interactive TUI behavior. A piped or detached invocation can follow a different client path from a real terminal session. I therefore recorded the observed stop while preserving the missing TTY as a limit on any broader claim about interactive enforcement.

### Interpreting the observed interactive outcome

The non-print run returned Error: Exceeded USD budget (0.0001) before producing a model answer. No CONTROL CHECK. response appeared, so the flag was active rather than ignored during this invocation.

This contradicted the help text as a broad safety description because that text said the control worked only with --print. In build 2.1.252, the cap also stopped non-print startup. The response shape differed from print mode: stderr carried a plain error instead of the machine-readable error_max_budget_usd result envelope.

The conclusion remained limited by the environment. Because no TTY was attached, the evidence proved non-print enforcement but did not establish how the full terminal interface handled the same flag. A real TTY run was still required to determine whether the setting applied there or whether this stop resulted from the noninteractive input path.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/492461ae-dceb-4e68-a5d2-065a101ebc8c_hhbfstbj)

## Reaching an Evidence-Backed Control Verdict

### Reconciling the plan, evidence, and observed outcomes

I compared the completed runs with the decision criteria and boundaries written before execution. The print-mode test showed that the cap stopped the client and returned a machine-readable error_max_budget_usd subtype after the locally estimated total crossed the configured limit.

The non-print run also stopped, even though the help text described the flag as print-only. That divergence did not erase the observed control behavior, but it left the documented scope inconsistent with the tested client path.

I also checked the pre-registered fields against the result schema. The expected stop-text field was absent in Claude Code 2.1.252, so that part of the planned evidence could not be collected. I preserved the mismatch as a divergence note instead of rewriting the original criteria. These findings supported WATCH rather than a stronger adoption or rejection decision.

### Selected verdict and falsifiable reversal condition

I selected WATCH. The cap stopped both tested runs, and print mode returned the machine-readable error_max_budget_usd subtype. However, the help text said print-only while the non-print invocation also stopped, and the pre-registered result stop-text field did not exist in version 2.1.252.

The verdict would change to REJECT if a repeatable run showed that the cap failed to stop execution at all. That outcome would break the acceptance criteria instead of exposing only a documentation or schema mismatch.

The verdict would change to ADOPT if a later build’s documented scope matched behavior confirmed in a real TTY and restored a stop-text field. I scheduled the next check for 2026-12-02 or the next minor release, whichever arrived first. Until then, the control remained useful but not settled enough for full reliance.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/492461ae-dceb-4e68-a5d2-065a101ebc8c_4r11jni5)

## Challenging the Verdict With Contrary Evidence

### Defining the observable trigger for a different decision

The contrary test required a real attached terminal. I would run claude --max-budget-usd 0.0001, submit Reply with exactly CONTROL CHECK., and repeat the test three times under the same TTY condition.

If all three attempts returned normal model answers, the replacement verdict would be ADOPT. That result would show that the flag was inactive in the full TUI, the documented print-only boundary held, and the earlier non-print stop came from the piped-input path. The missing result stop-text field would remain the only recorded gap.

If the TTY session stopped on the cap, WATCH would remain. That outcome would confirm that enforcement extended beyond the documented print-only scope and would turn the help-text mismatch into a reportable client defect. Three repeatable attempts created a visible trigger rather than allowing one unusual run to decide the verdict.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/492461ae-dceb-4e68-a5d2-065a101ebc8c_9erxi6g1)

## Establishing a Subscription-Only Windows Baseline

### Verifying the timed local test environment

I verified the local Claude Code version, Windows environment, and authentication path before testing the budget flag. This established the client and credential conditions under which the later results were produced.

The test used Claude Code 2.1.252 on Windows with subscription authentication. I checked that ANTHROPIC_API_KEY was absent so the invocation could not silently use that environment variable for pay-as-you-go API billing.

I also tracked elapsed time and captured the output files needed for the evidence packet. These checks separated the budget-control result from unrelated setup changes or credential routing. The baseline did not prove that the run stayed within a time or token limit. Those claims required their own wall-clock and usage measurements. It established only the local client, operating system, and subscription-only path used by the bounded run.

### Confirming the next run uses subscription authentication

The missing ANTHROPIC_API_KEY established credential precedence for the next run. Without that environment variable, Claude Code could not silently route the invocation through pay-as-you-go API billing using a supplied API key.

The print-mode request therefore used the existing subscription credential. This made the measured usage attributable to the same Pro, Max, Team, or Enterprise allowance path used by the authenticated client rather than a separate API-key account.

The evidence did not identify the exact plan tier or prove anything about the final invoice. It also did not show that the run stayed below 30 minutes or under a token ceiling. Those questions depended on separate elapsed-time and usage records. The authentication check supported one limited conclusion: the budget flag was exercised on a subscription-backed Windows session with no API key present.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/492461ae-dceb-4e68-a5d2-065a101ebc8c_lwihrdzv)

## Pre-Registering the Test and Its Boundaries

### Creating decision criteria before execution

I wrote the run plan, decision criteria, evidence fields, and verdict rules before executing the budget tests. This prevented the observed results from defining their own acceptance standard after the client had already stopped or continued.

The plan specified the cap, client mode, authentication condition, output format, required fields, and arithmetic used to calculate the difference between the reported total and configured limit. It also declared what evidence would support ADOPT, WATCH, or REJECT.

When the result schema lacked the expected stop-text field and non-print behavior differed from the help text, I appended those divergences instead of changing the original plan. This preserved the difference between what I expected and what version 2.1.252 returned. The final verdict could therefore be traced to rules written before the evidence existed.

### Limiting claims to local estimates and tested Windows behavior

The interpretation rules limited the result to a local computed-cost control. The cap, total_cost_usd, and calculated difference were client-side estimates, so a stop proved that Claude Code halted locally after comparison. It did not prove that billing was blocked upstream.

The tested platform was Windows. I made no claim that macOS or Linux would return the same result, error form, timing, or enforcement boundary. Those operating systems were outside the evidence.

Authentication was fixed to the existing subscription with no API key present. I also kept the version 2.1.239 reference separate as an estimate-calculation change. That earlier change could not explain the observed difference between the cap and reported total in version 2.1.252. These limits kept the findings tied to the client, version, platform, mode, and credential path that were measured.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/492461ae-dceb-4e68-a5d2-065a101ebc8c_p8bzwxfh)

## Defining the Operational Decision

### Who can rely on this control and what the evidence must decide

I defined one operational question: whether Claude Code’s --max-budget-usd flag behaved reliably enough to serve as a dependable local cost-control measure in my Windows print-mode workflow.

The evidence needed to show whether the client stopped after crossing the configured limit, how the reported total compared with the cap, and whether the result exposed a machine-readable failure type. It also needed to separate local estimates from authoritative billing.

The print-mode run showed a reliable local stop and returned error_max_budget_usd, but it exceeded the cap before halting. The non-print result also enforced the limit despite the help text describing print-only scope. Because the stop worked while its scope and schema remained unsettled, the decision was WATCH. This control could bound local execution, but it was not a strict upstream spending gate or a settled cross-mode contract.

## Reflecting on the Control Characterisation

### Tools and operational-control concepts applied

I used Claude Code with --max-budget-usd and --output-format json, plus PowerShell Tee-Object, ConvertFrom-Json, [decimal] arithmetic, New-TimeSpan, and Select-String. I also used claude --help to record the documented flag scope.

The main method was pre-registration. I wrote the verdict rules and run plan before gathering evidence, then appended divergence notes when the observed schema and mode behavior differed from those expectations.

The tests showed that the cap operated as a post-hoc trip wire on local estimates. The reported total reached about 9.7 times the cap before the client stopped. They also showed that documented scope and observed behavior differed in the non-print path. The missing TTY remained attached to that evidence as a limit, preventing the result from becoming a full-TUI claim.

### Project duration and key challenge

This build took approximately 30 minutes. The hardest part was mapping every statement in the final report to a local file, raw JSON value, help entry, terminal message, timestamp, or calculation.

The cap and total came from separate values, while the difference came from decimal subtraction. The print-mode result used a JSON envelope, but the non-print stop appeared as plain stderr. Treating those forms as interchangeable would have hidden the mode difference.

I also needed to preserve limits around billing, authentication, operating system, and TTY state. The evidence showed local stopping behavior, not authoritative charges or cross-platform consistency. Building the audit trail required more than recording that an error appeared. Each conclusion had to state which client path produced it, what the measurement proved, and what remained untested.

### Next steps in practical control testing

I completed this build to understand how Claude Code’s --max-budget-usd control behaved on Windows under subscription authentication. The print-mode run stopped with a machine-readable subtype after the local estimate crossed the cap, while the non-print run stopped through plain stderr.

The result supported WATCH because enforcement worked, but the help scope, missing stop-text field, overshoot behavior, and lack of a real TTY test remained unresolved.

My next step is to run the predeclared three-attempt test in an attached terminal and repeat the same measurement on other operating systems. I also want to automate capture of the cap, reported total, difference, result subtype, stderr, version, mode, and elapsed time. Each platform should keep its own evidence rather than inheriting conclusions from the Windows run.

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/492461ae-dceb-4e68-a5d2-065a101ebc8c)*
