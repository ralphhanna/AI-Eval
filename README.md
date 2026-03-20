# AI-Eval

AI-Eval is a prompt-based framework for evaluating software repositories with AI.

It is designed to assess visible repository quality in a structured, evidence-based way and produce both machine-readable and human-readable reports.

The framework focuses on repository qualities such as:
- simplicity
- design quality
- test quality
- resilience / failure handling
- logging / observability
- security
- API contract quality
- dependency health
- documentation
- aggregate overall quality

AI-Eval is meant for **code review and technical assessment**, not for replacing static analysis, test execution, or a full security audit.

## What AI-Eval Produces

For a repository under review, AI-Eval should produce:
- one JSON result per evaluation dimension
- one Markdown report per evaluation dimension
- one consolidated `evaluation.json`
- one consolidated `evaluation.md`

Optional:
- a chart or summary visual based on the final scores
- diff-only assessment when evaluating changes between versions

## Repository Structure

```text
01-system.txt
02-simplicity.txt
03-design-quality.txt
04-test-quality.txt
05-resilience.txt
06-logging.txt
07-security.txt
08-api-contract-quality.txt
09-dependency-health.txt
10-documentation.txt
11-aggregate.txt
12-diff-mode.txt
README.md
examples/
docs/
```

## Prompt Roles

### `01-system.txt`
This is the **core evaluator contract**.

It defines:
- evaluator role
- evidence discipline
- anti-hallucination rules
- confidence expectations
- output schema
- scoring behavior

This file is not end-user documentation. It is the reusable base prompt that should be applied together with the dimension prompts.

### `02` to `10`
These are the **dimension prompts**. Each prompt evaluates one quality area.

### `11-aggregate.txt`
Combines the dimension results into an overall assessment.

### `12-diff-mode.txt`
Used when comparing changes rather than evaluating a full repository from scratch.

## Recommended Evaluation Flow

### Full repository evaluation
Use this order:

1. `01-system.txt`
2. `02-simplicity.txt`
3. `03-design-quality.txt`
4. `04-test-quality.txt`
5. `05-resilience.txt`
6. `06-logging.txt`
7. `07-security.txt`
8. `08-api-contract-quality.txt`
9. `09-dependency-health.txt`
10. `10-documentation.txt`
11. `11-aggregate.txt`

### Diff / change evaluation
Use this order:

1. `01-system.txt`
2. `12-diff-mode.txt`
3. relevant dimension prompts as needed
4. `11-aggregate.txt`

## Evaluation Rules

AI-Eval should be used with these operating rules:

- Judge only what is visible in the provided repository, diff, and supporting files.
- Be conservative when evidence is weak.
- Do not invent runtime behavior, coverage, security posture, or architecture not shown by evidence.
- Distinguish clearly between findings, risks, and confirmed evidence.
- Prefer specific repository evidence over generic advice.
- Keep scoring consistent across dimensions.
- Avoid double-counting the same issue under multiple categories when possible.

## Recommended Output Shape

Each dimension should produce:

```json
{
  "category": "simplicity",
  "score": 7.2,
  "confidence": "medium",
  "summary": "Focused codebase with some oversized orchestration points.",
  "strengths": ["..."],
  "weaknesses": ["..."],
  "findings": [
    {
      "title": "...",
      "severity": "medium",
      "evidence": ["..."]
    }
  ],
  "recommendations": ["..."]
}
```

The aggregate output should summarize the dimension results and provide an overall score and top priorities.

## How to Use with ChatGPT

### Option A: Evaluate a repository attached as ZIP
1. Attach the repository ZIP.
2. Attach or paste the AI-Eval prompt files.
3. Ask ChatGPT to evaluate the repository using `01-system.txt` plus each dimension prompt.
4. Request:
   - one JSON file per dimension
   - one Markdown file per dimension
   - `evaluation.json`
   - `evaluation.md`

Example prompt:

```text
The attached ZIP is the target repository.
Use the AI-Eval prompt set in this folder.
Apply 01-system.txt as the common evaluator contract, then run prompts 02 through 10, then 11-aggregate.txt.

Produce:
- one JSON output for each dimension
- one Markdown report for each dimension
- one consolidated evaluation.json
- one consolidated evaluation.md

Be evidence-based and conservative. Do not assume test execution, real coverage, or hidden architecture.
```

### Option B: Evaluate a public GitHub repository
Provide:
- repository URL
- AI-Eval prompt set
- same output request as above

Example prompt:

```text
Use the AI-Eval prompt set in this folder to evaluate this repository:
https://github.com/OWNER/REPO

Apply 01-system.txt as the common evaluator contract, then run prompts 02 through 10, then 11-aggregate.txt.
Produce:
- one JSON output for each dimension
- one Markdown report for each dimension
- one consolidated evaluation.json
- one consolidated evaluation.md

Use repository evidence wherever possible. Be conservative when evidence is weak.
```

## Notes on Scope

AI-Eval is useful for:
- prototype reviews
- engineering due diligence
- architecture/code quality snapshots
- pre-refactor assessments
- PR / diff quality reviews

AI-Eval does **not** by itself guarantee:
- true code coverage measurement
- runtime correctness
- performance profiling
- penetration testing
- dependency vulnerability scanning

Those require additional tools and direct execution or scanning.

## Output Naming Convention

Recommended file names:

```text
02-simplicity.json
02-simplicity.md
03-design-quality.json
03-design-quality.md
04-test-quality.json
04-test-quality.md
05-resilience.json
05-resilience.md
06-logging.json
06-logging.md
07-security.json
07-security.md
08-api-contract-quality.json
08-api-contract-quality.md
09-dependency-health.json
09-dependency-health.md
10-documentation.json
10-documentation.md
11-aggregate.json
11-aggregate.md
evaluation.json
evaluation.md
```

## Example

See the `examples/` folder for sample outputs.

## Future Improvements

Planned improvements for the framework may include:
- stronger boundary rules between dimensions
- a single master orchestration prompt
- more stable scoring guidance
- repo-type normalization rules
- optional weighting outside the model

---

AI-Eval works best when used as a disciplined review framework: visible evidence in, structured judgment out.
