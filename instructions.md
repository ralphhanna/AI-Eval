# AI-Eval Instructions

## Purpose

Use this framework to evaluate a software repository across multiple engineering quality dimensions and produce one final consolidated report.

## Final Required Outputs

Save results inside the target project folder under:

- `AI-Eval-Results/evaluation.json`
- `AI-Eval-Results/evaluation.md`

Do not create separate result files per dimension unless explicitly requested.

## Evaluation Dimensions

Run the evaluation in this order:

1. Simplicity
2. Design Quality
3. Test Quality
4. Error Handling
5. Exception Handling
6. Logging / Observability

Then produce:

7. Overall Assessment and prioritized recommendations

## General Rules

- Evaluate the actual repository contents, not assumptions.
- Use file-level or code-level evidence wherever possible.
- If evidence is missing, say so clearly.
- Do not guess metrics such as coverage percentage.
- Do not confuse absence of evidence with proof of poor quality.
- Prefer concrete findings over generic advice.
- Keep scoring consistent across dimensions.
- Produce one consolidated `evaluation.json` and one readable `evaluation.md`.

## Expected Inputs

When possible, inspect:

- source code
- test files
- package manifests
- build files
- configs
- infrastructure definitions
- docs / README
- CI/CD workflows

If the repository does not use a standard structure such as `src/`, evaluate the structure that exists.

## Scoring

Use a score from 0 to 10 for each dimension.

Suggested interpretation:

- 9 to 10: excellent
- 7 to 8: strong
- 5 to 6: mixed / acceptable
- 3 to 4: weak
- 0 to 2: severely deficient

Each score must be justified by evidence.

## Per-Dimension Output Requirements

Each dimension result must include:

- `dimension`
- `score`
- `summary`
- `strengths`
- `weaknesses`
- `findings`
- `recommendations`
- `evidence`
- `confidence`

## Field Guidance

### dimension
- stable machine-friendly identifier
- examples: `simplicity`, `design_quality`, `test_quality`

### score
- numeric score from 0 to 10
- decimals allowed if needed

### summary
- short paragraph summarizing the overall judgment for that dimension

### strengths
- list of notable positives

### weaknesses
- list of notable negatives

### findings
- list of concrete observations
- each finding should be a structured object with:
  - `title`
  - `severity`
  - `description`
  - `affected_files`
  - `rationale`

### recommendations
- list of clear actionable improvements
- each recommendation should be a structured object with:
  - `id`
  - `priority`
  - `text`

### evidence
- list of structured repository references supporting the assessment
- each evidence item should include:
  - `file`
  - `note`

### confidence
- `high` / `medium` / `low`
- based on how complete and reliable the evidence is

## Evidence Standards

Use evidence such as:

- file names
- module structure
- presence or absence of tests
- implementation patterns
- catch blocks
- logger usage
- controller/service size
- dependency layout
- CI workflow contents

Avoid vague statements such as:

- "seems scalable"
- "probably well tested"
- "likely maintainable"

Instead use statements such as:

- "The repository contains X test files under Y"
- "A controller file exceeds typical single-responsibility boundaries"
- "Several empty catch blocks suppress exceptions without remediation"

## Dimension-Specific Notes

### Simplicity
Assess clarity, complexity, readability, layering, and whether the implementation feels heavier than necessary.

### Design Quality
Assess architecture, modularity, cohesion, coupling, separation of concerns, reuse patterns, and maintainability.

### Test Quality
Assess existence, spread, organization, realism, coverage intent, and quality of test structure.

Do not invent numerical coverage.

### Error Handling
Assess validation, fallback behavior, recoverable failures, guard clauses, returned error states, status handling, and resilience for expected failure paths.

### Exception Handling
Assess thrown exceptions, `try/catch` usage, rethrow patterns, swallowed exceptions, exception translation, cleanup, and transformation into domain-safe responses.

### Logging / Observability
Assess structured logging, visibility into failures, diagnostics, traceability, and operational usefulness.

## Consolidation Rules

After all dimension evaluations are complete:

1. Merge all dimension outputs into one final `evaluation.json`
2. Produce one readable `evaluation.md`
3. Ensure dimension names and score keys are consistent
4. Deduplicate overlapping recommendations where possible
5. Produce a separate overall assessment in the `overall` section, not as another dimension



## Required evaluation.json Structure

Write one consolidated `evaluation.json` using this normalized structure:

```json
{
  "project": {
    "name": "project-name",
    "path": "./"
  },
  "evaluation": {
    "tool": "AI-Eval",
    "date": "YYYY-MM-DD"
  },
  "dimensions": [
    {
      "dimension": "simplicity",
      "score": 8,
      "confidence": "high",
      "summary": "Short summary.",
      "strengths": ["Strength 1", "Strength 2"],
      "weaknesses": ["Weakness 1", "Weakness 2"],
      "findings": [
        {
          "title": "Large orchestration files",
          "severity": "medium",
          "description": "Some modules combine multiple responsibilities.",
          "affected_files": ["src/example.ts"],
          "rationale": "This increases cognitive load and reduces clarity."
        }
      ],
      "recommendations": [
        {
          "id": "R1",
          "priority": "high",
          "text": "Split large orchestration modules into focused units."
        }
      ],
      "evidence": [
        {
          "file": "src/example.ts",
          "note": "Contains multiple responsibilities in one module."
        }
      ]
    }
  ],
  "overall": {
    "score": 7.4,
    "confidence": "medium",
    "summary": "Overall assessment.",
    "top_strengths": ["Top strength 1", "Top strength 2"],
    "top_risks": ["Top risk 1", "Top risk 2"],
    "priority_recommendations": [
      {
        "id": "R1",
        "priority": "high",
        "text": "Split large orchestration modules into focused units."
      }
    ]
  }
}
```

## Markdown Report

Write `evaluation.md` as a readable report that includes:

- project name
- evaluation date
- overall score
- overall summary
- scores by dimension
- key strengths
- key risks
- priority recommendations
- a section for each dimension with its summary, findings, and recommendations
- recommendations listed by priority and numbered as `R#`
- final conclusion

## Making Decisions

- Make concrete actional recommendation to improve for each category
- The user may later state `fix R5` to follow recommendation `R5`
- Recommendation IDs must therefore be stable and explicit
- Once recommendations are implemented, revise `evaluation.md` and `evaluation.json`
- Recommendations are identified by a number, for example: "R5" , so user can say `fix R5`
- When Using git, create a seperate branch for fixing recommendations
- Once fix is done mark it on the output evaluation.md as completed

## Handling Missing Evidence

If a repository lacks a dimension almost entirely:

- do not fabricate evidence
- explicitly state limitations
- lower confidence
- score based on observed reality, not possible hidden quality

## Output Quality Standard

The final result should be:

- specific
- evidence-based
- readable
- actionable
- consistent
