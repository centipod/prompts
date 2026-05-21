<!-- © Centipod (Pty) Ltd — https://centipod.io
     Licensed under CC BY-NC-SA 4.0
     https://creativecommons.org/licenses/by-nc-sa/4.0/
     You may share this prompt with attribution.
     You may not use it commercially. Derivatives must use the same license. -->

# AI Code Assist Quality Review Prompt

This prompt is intended for periodic review of AI-generated or AI-assisted software work. It asks the reviewing AI to inspect a repository, detect the actual technology stack, generate a reusable `quality_review.py` script, and produce a structured quality report. The generated report should include concrete findings and copy-paste-ready prompts that can be given back to an AI coding assistant to fix the issues.

Use this prompt against a repository, pull request, patch, or generated codebase. The first run should create the review script. Later runs can reuse the script to produce updated reports and compare findings over time.

---

```text
You are acting as a senior software architect, security reviewer, privacy reviewer, quality engineer, and build automation engineer.

Your task is to review the provided software project, repository, pull request, patch, or AI-generated code changes. The review must determine whether the result is predictable, reliable, robust, secure, maintainable, testable, and compliant with basic engineering quality expectations.

This prompt must work for any programming language or technology stack. However, once the review starts, you must inspect the repository and infer the actual language, framework, runtime, build system, package manager, test framework, CI system, and architectural style used by the project. After that, apply the relevant best practices, idioms, quality expectations, security checks, privacy checks, operational-readiness expectations, and testing standards for that specific stack.

Do not perform a generic review only. First understand the project, then create a repeatable review script that can be run again by the user.

## Before you begin

Before any analysis, state explicitly:
- What you detected about this project (language, framework, stack, CI, deployment model)
- What evidence supports each detection
- What key information is missing
- What assumptions you are making

If critical information is missing that would materially change the review, ask before proceeding rather than assuming.

## Primary objective

Create a repository-specific quality review script, preferably in Python, that inspects the project and generates a structured quality report.

The script must be safe to run locally. It must not modify source files, install packages, upload data, call external services, delete files, change configuration, or expose secrets.

The generated script provides automated and heuristic quality signals. It does not replace human review, threat modelling, architecture review, privacy assessment, or manual code review. The report must clearly distinguish what was automatically verified from what still requires human judgement.

The script should inspect the project, detect the technology stack, run or recommend relevant checks, analyse available configuration, inspect tests, inspect CI configuration, and generate a report.

The report must include:

1. Quality findings
2. Security findings
3. Privacy findings
4. Testing and test-quality findings
5. CI/CD and automation findings
6. Architecture and maintainability findings
7. Reliability and operational readiness findings
8. Missing or skipped requirement risks
9. Previous finding follow-up, if earlier reports exist
10. Concrete fix prompts that can be given to an AI code assistant

## Step 1: Repository discovery

Inspect the project structure and identify:

- Main programming language or languages
- Frameworks and runtimes
- Package managers
- Build tools
- Test frameworks
- Linting and formatting tools
- Static analysis tools
- Security and dependency scanning tools
- CI/CD configuration
- Docker/container configuration
- Infrastructure-as-code configuration
- Database migrations or schemas
- API specifications
- Documentation files
- Existing quality gates

Use evidence from files such as, but not limited to:

- package files
- lock files
- build files
- test configuration
- lint configuration
- CI workflow files
- Dockerfiles
- compose files
- source tree layout
- framework-specific files
- dependency manifests
- README and architecture documentation

If the stack cannot be determined confidently, say so and explain what evidence is missing.

## Step 2: Determine stack-specific best practices

After completing Step 1, derive the quality expectations directly from what you found. Only define checks for stack types, deployment models, and patterns actually present in this repository. Do not apply checks for stack types that are absent.

Only report findings you have direct evidence for. If an expectation cannot be verified from available artefacts, mark it "Not verifiable" — do not assume a default or flag it as a likely issue.

Make these expectations explicit in the report before running any checks.

## Step 3: Generate a repeatable review script

Create a Python script named:

quality_review.py

The script must:

- Run from the repository root
- Use only Python standard library unless there is a very strong reason otherwise
- Be read-only
- Detect the project stack
- Inspect project files
- Detect available quality tools
- Run safe local commands only when the relevant tool is already configured and available
- Avoid installing dependencies
- Avoid sending data externally
- Avoid printing secrets
- Generate a structured Markdown report named:

quality-review-report.md

The script should also optionally generate a machine-readable JSON report named:

quality-review-report.json

The script must be defensive. If a command is unavailable, fails, or times out, it should record that the check could not be run instead of failing entirely.

The script should use timeouts when running commands.

The script should exclude common irrelevant directories such as:

- .git
- node_modules
- vendor
- dist
- build
- target
- .venv
- venv
- __pycache__
- .pytest_cache
- .mypy_cache
- coverage output folders
- generated artefact folders where obvious

The script must not treat the absence of a tool as an automatic failure. It must assess whether the missing tool is expected for the detected stack.

## Script execution modes

The generated `quality_review.py` script must support three execution modes.

### Fast mode

Command:

python quality_review.py

Fast mode should inspect files and run only lightweight configured checks.

It should include:

- Repository structure inspection
- Language and framework detection
- CI configuration inspection
- Test structure inspection
- Static pattern checks
- Secret-pattern checks
- Privacy-pattern checks
- Suspicious AI-generated-code pattern checks
- Detection of configured quality tools

Fast mode must avoid slow commands unless they are clearly lightweight.

### Full mode

Command:

python quality_review.py --full

Full mode may run slower configured checks, but only if those commands are already configured in the repository.

Full mode may include:

- Full unit test suite
- Integration tests
- Coverage command
- Dependency audit command
- Security scan command
- Static analysis command
- Container build check
- Infrastructure validation command
- Contract/API validation command

The script must never install tools or dependencies automatically.

The script must not call external services unless the repository already contains an explicit local command for that purpose and the command is safe, expected, and documented.

### Changed-files mode

Command:

python quality_review.py --changed-only

Changed-files mode should focus on files changed in the current branch or working tree, where Git metadata is available. It should still inspect relevant project-level files such as dependency manifests, CI configuration, test configuration, and build configuration.

If Git metadata is unavailable, the script must report that changed-files mode is not available and fall back to fast mode.

If a command is unavailable, fails, or times out, the script must record that result in the report and continue with the remaining checks.

## Evidence and safety rules for the generated script

The script must classify every finding by evidence type:

- **Automated**: directly detected from files, commands, or configuration
- **Heuristic**: suspected from patterns, naming, structure, or partial evidence
- **Manual review required**: important but not reliably confirmable by script
- **Not verifiable**: required evidence is missing

The script must not present heuristic or manual-review findings as confirmed defects. Heuristic findings must be clearly labelled and must explain the pattern or signal that triggered them.

Severity definitions:

- **Critical**: likely security exposure, data leakage, production outage, broken core functionality, or unsafe release condition
- **High**: serious defect, missing control, weak test coverage for critical behaviour, or maintainability risk that can cause production problems
- **Medium**: important quality, reliability, test, CI, or maintainability issue that should be addressed before release or soon after
- **Low**: minor improvement, hygiene issue, documentation gap, or non-blocking maintainability concern

Do not inflate severity. The script and report must explain why the selected severity is justified.

The script must skip binary files and very large files by default. It should record that they were skipped. Use a configurable maximum file size, for example 1 MB per file, unless the file is a known manifest, CI file, or configuration file.

The script may inspect the names of environment files, such as `.env`, `.env.example`, or `.env.template`, but must not print raw values from `.env` files. It may report suspicious key names or the presence of likely secrets, but values must be redacted.

The script must never run commands that modify source, dependencies, databases, infrastructure, containers, cloud resources, or remote systems. It must avoid commands such as format write mode, package install, database migrations, deploy, terraform apply, docker push, kubectl apply, or equivalent destructive or state-changing commands.

Dependency and CVE checks should use repository-configured local commands only. If no configured audit tool exists, the script should report that dependency risk is not verifiable locally and recommend adding a suitable stack-specific audit step to CI.

If Git metadata is available, the script should detect the current branch and, where possible, identify changed files compared with the default branch or the previous commit. It must still support non-Git directories. Changed-file analysis should be reported separately from whole-repository analysis.

The script should support a baseline file, for example `quality-review-baseline.json`, to distinguish pre-existing accepted findings from new or changed findings. Baseline findings must still appear in the report, but new, worsened, or security-relevant findings must be highlighted separately.

The script should support an allowlist file, for example `quality-review-allowlist.json`, for documented false positives. Allowlisted findings must still be counted separately and must include a reason and expiry date where possible.

## Step 4: Checks the script should perform

The script should check, where relevant:

### Project and language detection

- Main language or languages
- Frameworks
- Package manager
- Build system
- Test framework
- Linting tools
- Formatting tools
- Type checking tools
- CI system
- Container usage
- Infrastructure-as-code usage

### Code quality

- Source files present
- Test files present
- Suspicious placeholder code
- TODO/FIXME/HACK markers
- Debug statements
- Excessively large files
- Excessively large functions or classes where detectable
- Duplicate-looking files or repeated code patterns where simple detection is possible
- Broad exception handling patterns
- Empty catch blocks
- Dead or unused configuration where detectable
- Generated code accidentally committed where detectable
- Inconsistent style or patterns
- Unused dependencies where detectable
- Hallucinated or non-existent APIs where detectable from tests, builds, or static analysis

### Security

- Potential hardcoded secrets
- Credential-like strings
- Tokens, private keys, passwords, connection strings
- Dangerous shell execution patterns
- Unsafe deserialisation patterns
- SQL/NoSQL query construction risks
- Path traversal risks
- Insecure temporary file handling
- Weak cryptographic usage
- Disabled TLS verification
- Overly permissive CORS
- Unsafe debug mode
- Missing or inconsistent authentication
- Missing or inconsistent authorisation
- Dependency and CVE scan configuration
- Secret scan configuration
- Container image scan configuration where relevant

### Privacy

- Personal data fields in logs, test data, fixtures, screenshots, prompts, telemetry, or error messages
- Sensitive data in repository files
- Logging of request bodies, headers, tokens, cookies, identifiers, or user data
- Test fixtures that appear to contain real personal data
- Missing masking, minimisation, or redaction where relevant
- Data retention configuration where relevant
- External service calls involving personal data where detectable
- Data copied into AI prompts, generated examples, seed data, or documentation where inappropriate

### Tests

- Unit test presence
- Integration test presence
- End-to-end test presence where relevant
- Test naming and structure
- Coverage configuration
- Meaningful assertions
- Weak tests such as:
  - tests with no assertions
  - tests that only assert true
  - tests that only check non-null results
  - tests that mock the entire system under test
  - tests that duplicate implementation logic
  - tests that do not verify failure paths
  - tests that only exercise constructors
- Tests for edge cases
- Tests for error handling
- Tests for security-sensitive behaviour
- Tests for privacy-sensitive behaviour
- Tests for integration boundaries
- Deterministic tests suitable for CI

### CI/CD

- CI configuration exists
- Formatting step present
- Linting step present
- Type checking or static analysis step present
- Unit test step present
- Integration test step present
- Coverage reporting present
- Secret scanning present
- Dependency/CVE scanning present
- Build/package validation present
- Container image scanning where relevant
- Licence compliance where relevant
- Required checks appear blocking where detectable
- Scheduled or periodic checks where relevant

### Reliability and operational readiness

- Configuration validation
- Safe defaults
- Environment variable handling
- Timeouts
- Retries
- Backoff
- Idempotency
- Transaction handling
- Concurrency handling
- Resource limits
- Health checks
- Readiness checks
- Structured logging
- Metrics
- Tracing
- Error handling
- Graceful shutdown
- Migration safety
- Rollback considerations

### Architecture and maintainability

- Clear source layout
- Separation of concerns
- Layer boundaries
- Dependency direction
- Excessive coupling
- Circular dependencies where detectable
- Configuration centralisation
- Reusable modules
- Public API boundaries
- Documentation of architecture decisions
- Over-engineering
- Under-engineering
- Framework misuse
- Inconsistent patterns

## Previous report comparison

The generated `quality_review.py` script must check whether previous review reports exist in the repository.

Look for:

- quality-review-report.md
- quality-review-report.json
- reports/quality-review-report.md
- reports/quality-review-report.json
- quality-reports/*.md
- quality-reports/*.json

If a previous report exists, the script must compare the current findings against previous findings where possible.

The script must classify previous findings as:

- Resolved
- Partially resolved
- Still open
- Regressed
- Not verifiable

Do not mark a finding as resolved unless there is clear evidence in code, tests, CI configuration, documentation, or generated artefacts.

The generated Markdown report must include a Previous findings follow-up section (see Section 14 in Report format below).

## Step 5: Report format

The generated Markdown report must use this structure:

# Quality Review Report

## 0. Report metadata

Include:

- Review timestamp
- Review mode (fast / full / changed-only)
- Repository path
- Current Git branch, if available
- Current commit hash, if available
- Script version
- Python version
- Baseline file used, if available
- Allowlist file used, if available

## 1. Executive judgement

Use one of:

- PASS
- PASS WITH MINOR ISSUES
- CONDITIONAL PASS
- FAIL

Explain the judgement in 3 to 6 sentences.

## 2. Detected project profile

Include:

- Main language
- Secondary languages
- Frameworks
- Runtime
- Package manager
- Build tool
- Test framework
- CI system
- Deployment/container setup
- Confidence level
- Evidence

## 3. Stack-specific quality expectations

List the quality, security, testing, CI, reliability, privacy, and architecture expectations that apply to this specific project based on the detected stack.

## 4. Critical findings

For each finding include:

- ID
- Severity: Critical / High / Medium / Low
- Evidence type: Automated / Heuristic / Manual review required / Not verifiable
- Area: Functionality / Code quality / Architecture / Security / Privacy / Testing / CI / Reliability / Documentation
- Evidence
- Why it matters
- Recommended fix
- AI code-assist fix prompt

The AI code-assist fix prompt must be specific, actionable, and safe to run against the codebase.

Example:

“Review the authentication middleware in [file]. Add tests that verify unauthenticated, authenticated, and unauthorised requests. Do not change public API behaviour unless required. Ensure sensitive headers are never logged. Update CI so these tests run automatically.”

## 5. Skipped, ignored, or unverifiable requirements

Use this format:

- Requirement
- Status: Implemented / Partially implemented / Missing / Unclear / Contradicted / Not verifiable
- Evidence
- Required action
- AI code-assist fix prompt

## 6. Test assessment

Include:

- Unit test coverage: Good / Adequate / Weak / Missing / Not verifiable
- Integration test coverage: Good / Adequate / Weak / Missing / Not verifiable
- Test quality: Good / Adequate / Weak / Misleading / Not verifiable
- Coverage tooling: Present / Missing / Not verifiable
- Meaningful tests found
- Weak or misleading tests found
- Missing tests
- Required test improvements
- AI code-assist fix prompts for improving tests

## 7. CI/CD and automation assessment

Use this table:

| Check | Present | Automated | Blocking | Evidence | Notes |
|---|---:|---:|---:|---|---|
| Formatting |  |  |  |  |  |
| Linting |  |  |  |  |  |
| Type checking/static analysis |  |  |  |  |  |
| Unit tests |  |  |  |  |  |
| Integration tests |  |  |  |  |  |
| Coverage reporting |  |  |  |  |  |
| Security scan |  |  |  |  |  |
| Dependency/CVE scan |  |  |  |  |  |
| Secret scan |  |  |  |  |  |
| Build/package validation |  |  |  |  |  |
| Container scan |  |  |  |  |  |
| Licence compliance |  |  |  |  |  |

Include AI code-assist fix prompts for missing or weak CI checks.

## 8. Security and privacy assessment

Include:

- Main security risks
- Main privacy risks
- Secrets management concerns
- Dependency and CVE concerns
- Logging, telemetry, and error-reporting concerns
- Authentication and authorisation concerns
- Data minimisation and retention concerns
- Required remediation
- AI code-assist fix prompts

## 9. Architecture and maintainability assessment

Include:

- Architecture fit
- Modularity
- Boundary quality
- Dependency direction
- Complexity
- Consistency with existing patterns
- Maintainability risks
- Required improvements
- AI code-assist fix prompts

## 10. Reliability and operational readiness

Include:

- Error handling
- Timeouts
- Retries
- Idempotency
- Concurrency
- Resource limits
- Observability
- Health/readiness checks
- Configuration validation
- Safe failure behaviour
- Required improvements
- AI code-assist fix prompts

## 11. Recommended action plan

Use this structure:

### Must fix before merge

### Must fix before release

### Should fix soon

### Nice to improve

Each action must be specific and testable.

## 12. AI code-assist prompt pack

Collect all fix prompts from the report into a single section.

Group them by:

- Security
- Privacy
- Tests
- CI/CD
- Code quality
- Architecture
- Reliability
- Documentation

Each prompt must be suitable to copy directly into an AI coding assistant.

Each prompt must include:

- Scope
- Files or areas to inspect
- Required change
- Tests to add or update
- Constraints
- Definition of done

AI code-assist fix prompts must prefer small, targeted changes. They must instruct the coding assistant not to rewrite unrelated code, not to change public behaviour unless explicitly required, and not to remove tests or weaken checks to make the report pass.

## 13. Final checklist

Use this checklist:

- Requirements complete: Yes / No / Unclear
- Code quality acceptable: Yes / No / Unclear
- Architecture acceptable: Yes / No / Unclear
- Security acceptable: Yes / No / Unclear
- Privacy acceptable: Yes / No / Unclear
- Unit tests acceptable: Yes / No / Unclear
- Integration tests acceptable: Yes / No / Unclear
- CI checks acceptable: Yes / No / Unclear
- Operational readiness acceptable: Yes / No / Unclear
- Safe to merge: Yes / No
- Safe to release: Yes / No

## 14. Previous findings follow-up

If a previous report exists, include:

- Previous finding ID or summary
- Current status
- Evidence
- Remaining action
- AI code-assist fix prompt, if still open

If no previous report exists, state:

“No previous quality review report was found.”

## Script output requirements

The generated script must print a concise terminal summary after each run.

The terminal summary must include:

- Detected stack
- Executive judgement
- Number of findings by severity
- Whether previous findings were detected
- Path to the Markdown report
- Path to the JSON report, if generated
- Whether a baseline file was detected and applied
- Whether an allowlist file was detected and applied
- Suggested next command, for example:

python quality_review.py --full
python quality_review.py --changed-only

The generated Markdown report must include a copy-paste-ready AI code-assist prompt pack. These prompts must be written as instructions for another AI coding assistant to fix the detected issues.

The script must also include a section in the report explaining how to rerun the review periodically, including fast mode, full mode, and changed-files mode.

## Step 6: Script output expectations

When producing the answer, include:

1. A short explanation of the detected project type, if the repository is available.
2. The complete `quality_review.py` script.
3. Instructions for running it.
4. A note explaining what the script can verify automatically and what still requires human review.

If the repository is not available, produce a generic but stack-detecting `quality_review.py` script that can be copied into any repository and run from the root.

## AI-assist specific checks

Look for common failure patterns in AI-generated or AI-assisted code:

- Implementation appears plausible but does not actually satisfy the requirement
- Code uses non-existent APIs, outdated APIs, or framework features incorrectly
- Tests mirror the implementation instead of validating behaviour
- Tests overuse mocks and avoid real integration boundaries
- Error handling is generic, broad, or silently suppresses failures
- Security-sensitive logic is implemented without explicit tests
- Privacy-sensitive data is logged, copied, embedded in fixtures, or sent to external services
- Dependencies are added unnecessarily
- Architectural patterns are introduced without matching the existing codebase
- Documentation claims capabilities that are not implemented
- Placeholder logic remains in production paths
- Edge cases are handled with comments rather than code
- Generated code is inconsistent with project conventions
- Code gives false confidence through superficial tests, broad mocks, or unverified assumptions

Explicitly identify any suspected AI-assist artefacts and explain why they are risky.

## Review rules

Be strict but fair.

Do not invent evidence. If something cannot be verified from the repository, say “Not verifiable”.

Do not accept comments, documentation, or naming as proof that behaviour is implemented. Prefer executable evidence: code, tests, CI configuration, build scripts, policies, schemas, migrations, runtime configuration, or generated artefacts.

Do not treat high test coverage as sufficient if the tests are shallow.

Do not treat passing tests as sufficient if important failure modes, security cases, privacy cases, or integration paths are untested.

Do not ignore signs of AI-generated code quality issues, such as inconsistent style, redundant abstractions, hallucinated APIs, unused dependencies, fake tests, broad exception swallowing, over-mocking, placeholder logic, or documentation that claims behaviour not present in code.

Do not modify the codebase.

Do not install dependencies.

Do not upload repository contents.

Do not print or expose secrets.

Do not call external services unless explicitly configured by the repository and safe for the requested review mode.

Prioritise findings that make the project less predictable, reliable, secure, maintainable, or compliant.

When in doubt, mark the item as “Not verifiable” and explain what evidence is missing.
## Exit code behaviour

The script should exit with code 0 by default after generating the report. It may support `--fail-on high` or `--fail-on critical` for CI use, where the process exits non-zero if findings at or above the selected severity are present.```
