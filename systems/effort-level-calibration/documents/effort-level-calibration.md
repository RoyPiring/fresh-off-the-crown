<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Calibrate Claude Code Effort

**Project Link:** [View Project](https://nextwork.ai/projects/fb55ac07-6305-42ce-b2b1-1a42a2716611)

**Author:** Roy Piring: Cloud Platform Engineer | Build Master  
**Email:** rpiringhawaii@gmail.com

---

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/fb55ac07-6305-42ce-b2b1-1a42a2716611_g2jhq972)

## Measuring a Script-Backed Effort Decision

### Running the isolated high-versus-medium comparison

I ran a controlled comparison between Claude Code high-effort and medium-effort repairs through benchmark.py. Each arm began from a fresh copy of the same broken invoice workspace and received the same prompt, pinned model, and 8-turn limit.

The two repairs ran in separate processes with distinct session IDs and no persistence between them. The only intended difference was the --effort value. This kept prior conversation state and workspace changes from crossing between the test arms.

The harness recorded the outputs, process results, turn counts, checker status, and evidence needed by the decision rule. It then produced a report limited to this invoice repair. The comparison measured whether medium was sufficient under the declared conditions. It did not claim that one effort level would perform the same way on every coding task.

### Why the verdict is evidence-based

I fixed the failures and decision rule before either model ran. preregistered_failures.md was hashed and copied to results/pre-artifacts/, while pre_artifacts_unchanged: True later confirmed that the faults and acceptance criteria had not moved.

The verdict came from decision_for(), which evaluated the written rule against the return code from check_invoice.py. The checker’s hash was verified unchanged in both arms, preventing either model from passing by editing its own test.

Both runs used byte-identical starting workspaces with hash 32ec72e7…, the same pinned model, prompt, and 8-turn cap. They ran as separate processes with different session IDs and no persistence. Because only --effort changed, the report could connect the observed difference to that setting within this narrow repair experiment.

## Proving the Decision Fails Closed

### Why worse lower-effort evidence retains high

The decision rule required lower["turns"] <= default["turns"] + 1. High used 7 turns, which set the allowed medium ceiling at 8. When the medium value was tampered to 9, that comparison returned false.

criterion_met used one bool(... and ...) chain containing every required condition. A single false clause made the full expression false, even when all other checks remained true. The rule did not assign partial credit or average a failed requirement with successful controls.

decision_for() returned "USE_MEDIUM" only when the entire chain passed. Every other outcome returned "KEEP_HIGH". This made KEEP_HIGH the fail-closed default produced by code, not a fresh judgment about the tampered run. The secret test proved that worse lower-effort evidence could not slip through because the other controls still passed.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/fb55ac07-6305-42ce-b2b1-1a42a2716611_4yddh3xm)

## Building an Auditable Comparison Harness

### Creating controlled, isolated repair runs

I built benchmark.py to create, execute, and score separate high-effort and medium-effort repair runs. The harness copied the same broken fixture into two fresh workspaces and verified that their starting hashes matched byte for byte.

Each copy had to fail check_invoice.py before repair. This confirmed that both models received the same unresolved faults and that neither arm began from a partly repaired state. The harness then launched the runs as separate processes with distinct session IDs.

The execution settings held the pinned model, prompt, and 8-turn cap constant while changing only --effort. Afterward, the harness ran the unchanged checker and collected the return code, turns, hashes, and control results. This structure made the comparison repeatable and allowed the final decision to be rebuilt from saved evidence rather than model summaries.

### Pre-run provenance and isolation controls

The access attestation recorded an eligible Max plan, Fable inclusion, disabled usage credits, subscription authentication, and a real checked_at value. A live scan also inspected 13 metered or routing variables plus CLAUDE_CODE_EFFORT_LEVEL.

That scan found ANTHROPIC_BASE_URL and stopped the first attempt before model execution. This prevented an unexpected route from entering the comparison and showed that the preflight gate could block a contaminated environment.

The preregistration contained all three mutation strings and the decision rule. It and the attestation were copied to results/pre-artifacts/ with SHA-256 hashes. Claude on PATH, version, and authentication checks passed. The broken fixture still failed, both fresh copies had matching workspace_hash values, and each independently failed before repair. These controls established provenance and starting parity.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/fb55ac07-6305-42ce-b2b1-1a42a2716611_b5zigpz9)

## Preregistering the Repair Experiment

### Defining the faults and acceptance rule in advance

I preregistered three code mutations against the invoice calculation and documented the test failures each mutation was expected to produce. This created the broken fixture before either Claude Code effort level attempted a repair.

I also wrote the acceptance rule in advance. The rule covered checker success, artifact integrity, workspace parity, turn limits, and the conditions required for medium effort to replace high effort for this specific task.

The preregistration was timestamped, hashed, and copied to results/pre-artifacts/ before the first model call. This ordering prevented me from selecting faults that favored the result or changing the success bar after seeing which arm performed better. The final run could only satisfy or fail conditions that already existed on disk, making the recommendation falsifiable and traceable to evidence created before execution.

### Preventing post-hoc test design

Choosing the three mutations first meant they represented the repair behaviors I intended to test. I could not redesign the faults around what high or medium happened to fix, overlook, or misunderstand after execution.

Writing the acceptance rule first removed the same freedom from the final decision. I could not redefine a passing repair, change the permitted turn difference, or reweight one result once I knew which effort setting benefited.

Together, the frozen mutations and rule made the comparison falsifiable. The models either repaired the declared faults within the declared controls or they did not. Hashes and pre-artifact snapshots made later changes detectable. This did not guarantee that the test covered every type of repair. It ensured that this invoice comparison answered the question defined before evidence existed rather than one rewritten to match the outcome.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/fb55ac07-6305-42ce-b2b1-1a42a2716611_rjtekpkd)

## Establishing a Verified Invoice Control

### Creating the known-good repair baseline

I created a known-good invoice calculation that used integer-cent arithmetic. This avoided using floating-point values as the reference for the business rules under test and gave the repair experiment a clear expected behavior.

I encoded the invoice rules in the function and wrote check_invoice.py to test the required results. The clean fixture passed before I created the mutated copy, confirming that the implementation and checker agreed under the intended conditions.

This known-good state served as the control for every later step. The three mutations deliberately moved the fixture away from it, and a successful repair had to restore behavior that passed the unchanged checker. The harness did not ask a model or reviewer whether the output looked correct. It used executable rules established before the comparison, giving “repaired” one consistent meaning across high and medium effort runs.

### Why the clean checker must pass first

A passing clean fixture proved that the three tests could recognize the intended invoice behavior before any mutation was introduced. If the baseline had failed, a later failure could have come from a broken assertion, import problem, or incorrect reference rather than the planted fault.

The clean pass also fixed the repair target in executable form. A model repair counted only when check_invoice.py returned success. This removed the need to judge the code by appearance or accept a plausible explanation without working behavior.

Running the checker first also confirmed that the interpreter, imports, files, and environment worked. The mutated copies could then be expected to fail for their planted reasons. This isolated the measured variable more clearly: the later comparison tested repair behavior under different effort levels instead of mixing model performance with unresolved setup noise.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/fb55ac07-6305-42ce-b2b1-1a42a2716611_m0egss4k)

## Gating Included Access and Local Tooling

### Confirming an eligible, non-metered experiment path

I audited the environment before any model call and saved the result in access-attestation.json. The record confirmed an eligible Max plan, included Fable access, disabled usage credits, subscription login, and the time recorded in checked_at.

I also scanned 13 variables associated with metering or routing, along with CLAUDE_CODE_EFFORT_LEVEL. The first attempt stopped when the scan found ANTHROPIC_BASE_URL, preventing an unexpected endpoint from affecting the benchmark.

The gate checked the local commands, versions, authentication state, and preregistration files before allowing the comparison to proceed. This gave the two repair arms the same access path and kept cost or routing differences from becoming hidden variables. The attestation documented the tested conditions. It did not make claims about plans, credits, or routes outside the captured environment.

### Captured Python, VS Code, and Claude Code versions

python3 --version returned Python 3.14.7. The full banner was 3.14.7 (tags/v3.14.7:823f032, Aug 5 2026, 10:51:32) [MSC v.1944 64 bit (AMD64)]. This recorded the interpreter used by benchmark.py and check_invoice.py.

code --version returned command not found. VS Code was not installed in the tested environment, so there was no editor version to capture. I preserved that absence instead of substituting another editor or inferred installation.

claude --version returned 2.1.252 (Claude Code). This identified the client used for both repair arms. Recording all three checks established the local tool state before execution. Python and Claude Code were available at the stated versions, while VS Code was absent. The benchmark depended on the first two tools and did not treat the missing editor as a failed control.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/fb55ac07-6305-42ce-b2b1-1a42a2716611_8nyjk19j)

## Setting a Narrow, Practitioner-Focused Goal

### Comparing one controlled repair without overgeneralizing

I compared high-effort and medium-effort Claude Code repairs on one controlled invoice fixture. The harness measured repair success, turn count, artifact integrity, workspace parity, and the evidence required by the prewritten decision rule.

The comparison was designed to answer a practitioner question: whether medium effort was sufficient for this specific repair under the same prompt, pinned model, 8-turn cap, and byte-identical starting state. It did not attempt to rank the effort settings across all coding work.

I limited the conclusion to the final benchmark report. A result from three planted invoice faults could not establish expected behavior for architectural design, unfamiliar repositories, multi-file changes, or other models. The value came from replacing a default assumption with a measured local decision. Future tasks would require their own fixtures, acceptance rules, and evidence before inheriting the same setting.

## Reflecting on the Calibration Workflow

### Tools and concepts applied

I used benchmark.py as the comparison harness, check_invoice.py as the executable repair checker, and local workspace fixtures as the controlled inputs. SHA-256 hashes protected the preregistration, attestation, checker, and starting workspaces.

The main method was a gated experiment. Preflight checks verified access and routing, while byte-identical copies isolated --effort as the intended difference. Separate processes and session IDs prevented state from carrying between arms.

I also implemented a fail-closed decision rule. Medium was selected only if every required condition passed; otherwise, the rule retained high. The tampered 9-turn test demonstrated that one failed clause overrode the remaining successful controls. These practices made the result auditable and kept safety as the default when evidence was incomplete or worse.

### Project duration and challenge

This build took approximately 40 minutes. The hardest part was making sure the harness isolated the high and medium runs while preserving enough evidence to audit every decision condition.

Both arms needed fresh workspaces with identical hashes, independent failing checks before repair, the same model and prompt, separate processes, distinct session IDs, and no persistence. The checker also had to remain unchanged so neither model could alter its own acceptance test.

I then verified the fail-closed behavior by changing the medium turn value to 9. With high at 7, the rule allowed no more than 8, so decision_for() returned KEEP_HIGH. Keeping these controls aligned required more care than launching two model calls. The comparison was only meaningful if the starting state, execution conditions, checker, and decision logic remained fixed.

### Next learning goal

I completed this build to create a repeatable way to calibrate Claude Code effort. The method replaced guesswork with a preregistered repair fixture, isolated runs, an unchanged checker, recorded hashes, and a machine-applied decision rule.

The result applied only to the tested invoice repair. It did not establish that medium or high would be the right setting for every repository, fault type, or architectural task. That limit was part of the evidence rather than an exception added afterward.

My next goal is to apply the same benchmark design to more complex multi-file architectural refactoring. Those tests will need broader fixtures, explicit cross-file invariants, fixed acceptance checks, and separate cost and turn evidence. I want each calibration to stay narrow enough that its recommendation remains defensible for the work it measured.

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/fb55ac07-6305-42ce-b2b1-1a42a2716611)*
