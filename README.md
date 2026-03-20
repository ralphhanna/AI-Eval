# AI-Eval

For AI tools like ChatGPT, start with [instructions.md](https://github.com/RepoGradeAI/AI-Eval/blob/main/instructions.md).

## Purpose

AI-Eval is a prompt-based framework for evaluating software repositories with AI.

It is designed to assess visible repository quality in a structured, evidence-based way and produce both machine-readable and human-readable reports.


AI-Eval is meant for **code review and technical assessment**, not for replacing static analysis, test execution, or a full security audit.

## Evaluation Scope

The framework supports **core dimensions** and **optional dimensions**.

### Core dimensions
These should be evaluated by default unless the user explicitly narrows scope:

1. Simplicity
2. Design Quality
3. Test Quality
4. Error Handling
5. Exception Handling / Resilience
6. Logging / Observability

### Optional dimensions

7. Security
8. Performance Readiness
9. Usability / Developer Experience
10. API Contract Quality
11. Dependency Health
12. Documentation Quality

### Project Notes
If project root folder has a file `.AI-Eval` it will be considered
Example:
```text
    No Performance Readiness
    No Usablity / Developer Experience
```
--------
-- Aggregate Overall Quality 

### Dff Mode

- Diff Mode

### Scoring rule
The final aggregate score must be based only on the dimensions actually evaluated.

The report must explicitly include:
- **Included dimensions**
- **Skipped dimensions**
- **Reason skipped** for each skipped optional dimension

Do not penalize a repository for dimensions that were not requested, not relevant, or not reasonably inferable from the repository and project notes.

### Evidence rule
Optional dimensions must follow the same evidence standard as core dimensions:
- evaluate actual repository evidence
- do not invent benchmarks, user satisfaction, adoption quality, or runtime metrics
- if evidence is limited, say so clearly

## What AI-Eval Produces

For a repository under review, AI-Eval should produce:
- one JSON result per evaluation dimension
- one Markdown report per evaluation dimension
- one consolidated `evaluation.json`
- one consolidated `evaluation.md`

Optional:
- a chart or summary visual based on the final scores
- diff-only assessment when evaluating changes between versions

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
