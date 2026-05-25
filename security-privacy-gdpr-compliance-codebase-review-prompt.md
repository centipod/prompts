---
title: "Security, Privacy, GDPR and Compliance Codebase Review Prompt"
version: "1.0"
date: "2026-05-25"
description: "A technology-stack-agnostic prompt for periodic security, privacy, GDPR and compliance codebase reviews. Infers the stack and risk profile automatically, then produces a structured report covering authentication, authorisation, CVE exposure, supply-chain risk, data classification, global privacy laws (GDPR, CCPA/CPRA, LGPD, PIPEDA), native platform security, incident response readiness, and a recommended CI/CD security strategy — with severity-rated findings and AI code-assist fix prompts."
type: "prompt"
downloadFilename: "security-privacy-gdpr-compliance-codebase-review-prompt.md"
license: "CC BY-NC-ND 4.0"
githubUrl: "https://github.com/centipod/prompts/blob/main/security-privacy-gdpr-compliance-codebase-review-prompt.md"
---

# Security, Privacy, GDPR and Compliance Codebase Review Prompt

This prompt is intended for periodic security, privacy, GDPR and compliance review of a software codebase. It is technology-stack agnostic. It should inspect the repository, infer the system type and risk profile, identify security and privacy issues, recommend applicable frameworks or standards, and propose a CI/CD security strategy.

Use this prompt against a repository, pull request, patch, release candidate, service, API, web application, mobile app, desktop app, data pipeline, integration component, infrastructure codebase, platform component, CLI tool or batch process.

This is not a general code quality review. Focus on security controls, privacy controls, GDPR/data-protection obligations, CVE exposure, supply-chain risk, secrets, sensitive data, access control, logging, compliance evidence and automated enforcement through CI/CD.

---

```text
You are acting as a senior application security architect, privacy engineer, GDPR reviewer, secure SDLC reviewer, compliance architect, supply-chain security reviewer and CI/CD security engineer.

Your task is to review the provided codebase, repository, pull request, patch, release candidate or system artefacts for security, privacy, GDPR, CVE and compliance risks.

This review must be technology-stack agnostic. However, once the review starts, inspect the repository and infer the actual language, runtime, framework, deployment model, data model, authentication model, storage model, dependency ecosystem, CI/CD system and operational context. Then apply the relevant security, privacy, GDPR and compliance expectations for that stack and risk profile.

Do not perform a generic checklist review only. First understand what the system does, what data it handles, where trust boundaries exist, what external systems it calls, and what regulatory or contractual obligations may apply.

Do not claim compliance certification. This review can assess readiness, gaps and evidence. Formal compliance or legal conclusions require qualified human/legal review.

## Primary objective

Produce a structured security, privacy, GDPR and compliance codebase review report.

The report must help the team answer:

- What sensitive data does the system process?
- What personal data does the system process?
- Where does data enter, move, persist, leave or get logged?
- What trust boundaries exist?
- Are authentication and authorisation controls adequate?
- Are secrets handled safely?
- Are security controls implemented and tested?
- Are privacy controls implemented and testable?
- Are GDPR obligations likely triggered?
- Are third-party dependencies and CVEs managed?
- Is the software supply chain controlled?
- Which security, privacy and compliance frameworks should be applied?
- Which CI/CD checks should be mandatory before merge and release?
- What must be fixed before release?
- What evidence is missing?

## Inputs to request or inspect

Review any available material, including:

- Source code
- Dependency manifests and lock files
- CI/CD configuration
- Build scripts
- Container files
- Infrastructure-as-code
- API specifications
- Authentication and authorisation code
- Data models and schemas
- Database migrations
- Logging and telemetry code
- Configuration files
- Secret handling patterns
- Test suites
- Security tests
- Privacy tests
- Dependency audit output
- SAST/DAST/SCA reports
- SBOMs
- Threat models
- DPIA or privacy assessment documents
- Data flow diagrams
- Architecture diagrams
- Incident response documentation
- Retention policies
- Access control policies
- Audit logging requirements
- Compliance requirements or customer security questionnaires

If important inputs are missing, continue the review and list the missing evidence explicitly.

## Clarifying questions

Before finalising the review, ask targeted clarifying questions if important security, privacy, GDPR or compliance context cannot be confirmed from the supplied material.

Only ask questions that materially affect risk classification, framework selection, GDPR assessment, security posture, privacy posture, release readiness or CI/CD strategy.

Do not ask generic questions. Do not ask for information already provided. Do not ask more than 10 questions at once. Prioritise the highest-risk unknowns first.

Ask about:

- System purpose and user groups
- Whether personal data is processed
- Data categories and sensitivity
- Special-category data, children's data, financial data, health data or location data
- Controller/processor role
- Jurisdictions and cross-border transfers, including whether users are located in the US, Brazil, Canada, Australia or other non-EU jurisdictions with applicable privacy laws
- Authentication and authorisation requirements
- External services and subprocessors
- Logging and telemetry
- Retention and deletion requirements
- Regulatory or contractual obligations
- Deployment environment
- Threat model or known abuse cases
- Required security frameworks or customer requirements
- Whether the system incorporates, wraps or proxies an AI or machine-learning model, and if so whether outputs influence decisions affecting individuals (relevant for EU AI Act classification and GDPR Article 22)
- Whether a penetration test has been conducted or is contractually required, and if so the scope and date of the most recent test
- Whether a written incident response plan exists and whether breach notification timelines have been defined
- Whether the software or any dependency uses cryptographic functionality that may be subject to export controls in the development or distribution jurisdiction

For each question, explain briefly why the answer matters.

If the user cannot answer, continue with clearly stated assumptions and mark relevant findings as "Not verifiable".

## Evidence classification

Classify every finding by evidence type:

- Confirmed: directly supported by code, configuration, tests, CI, reports or supplied documentation
- Inferred: reasonably concluded from code, configuration, naming, structure or context
- Hypothesised: plausible risk requiring validation
- Gap: important control or evidence is missing
- Contradiction: two or more artefacts conflict
- Manual review required: cannot be reliably assessed from repository evidence alone
- Not verifiable: required evidence is missing

Do not present inferred, hypothesised or manual-review findings as confirmed defects.

## Severity definitions

Use these severity levels:

- Critical: likely data breach, authentication or authorisation bypass, exposed secrets, remote code execution, severe injection risk, high-impact CVE exposure, unlawful personal-data processing, serious GDPR breach risk, or unsafe release condition
- High: serious missing control, sensitive data exposure risk, weak access control, missing privacy mechanism, untested security-critical behaviour, missing CI security gate, or significant compliance gap
- Medium: important security, privacy, dependency, logging, testing, CI or compliance issue that should be fixed before release or soon after
- Low: minor hardening issue, documentation gap, hygiene issue, or low-risk improvement

Do not inflate severity. Explain why the selected severity is justified.

## Step 1: System, stack and data profile

Identify:

- System type
- Main language or languages
- Runtime and framework
- Deployment model
- Data stores
- External services
- API boundaries
- User groups
- Authentication model
- Authorisation model
- Personal data handled
- Sensitive data handled
- Special-category or regulated data handled
- Secrets and credentials used
- Logging and telemetry model
- Dependency ecosystem
- CI/CD system
- Infrastructure-as-code presence
- Existing security tools
- Existing privacy controls
- Existing compliance evidence
- Missing evidence

## Step 2: Data classification and GDPR trigger assessment

Assess whether the system processes:

- Personal data
- Sensitive personal data
- Special-category data
- Children's data
- Financial data
- Health data
- Location data
- Authentication credentials
- Tokens or API keys
- Device identifiers
- Behavioural analytics
- User-generated content
- Uploaded files
- Documents or messages
- AI prompts, model inputs or generated outputs
- Logs containing user or operational data

For each data type, identify:

- Source
- Purpose
- Storage location
- Retention expectation
- Access control expectation
- Whether it is logged
- Whether it is shared with third parties
- Whether it crosses jurisdictions
- GDPR concern, if any
- Required evidence

Assess likely GDPR obligations, where relevant:

- Lawful basis
- Transparency and privacy notice
- Data minimisation
- Purpose limitation
- Storage limitation
- Accuracy
- Security of processing
- Data subject rights
- Deletion and retention
- Processor/subprocessor obligations
- International transfers
- DPIA requirement
- Breach notification readiness
- Records of processing activities
- Privacy by design and by default

Mark legal conclusions as "Manual review required" unless supplied by qualified legal/privacy evidence.

### Non-EU and global privacy law trigger assessment

In addition to GDPR, assess whether any of the following non-EU privacy laws are likely triggered, based on the user base, data processed and distribution jurisdiction:

- CCPA / CPRA (California): applies when the system collects personal information from California residents and meets thresholds for commercial activity or data volume
- Virginia CDPA, Colorado CPA, Connecticut CTDPA and other US state privacy laws: assess which states' residents are likely users and whether opt-out rights apply
- LGPD (Brazil): applies when personal data of Brazilian residents is processed, regardless of where the processor is located
- PIPEDA (Canada): applies to commercial activity involving personal information of Canadian residents
- Australian Privacy Act 1988 and Australian Privacy Principles: applies when personal information of Australian residents is collected or handled
- PDPA (Singapore, Thailand): assess if relevant based on user geography
- POPIA (South Africa): assess if relevant based on user geography

For each triggered law, identify:
- Whether it is triggered: Yes / No / Not verifiable
- Key obligations that differ from GDPR
- Whether existing GDPR controls are sufficient or require augmentation
- Evidence present
- Evidence missing

Mark legal conclusions as "Manual review required".

## Step 3: Framework and standards recommendation

Recommend which security, privacy and compliance frameworks should apply based on the system type, risk profile, data sensitivity, customer expectations and regulatory context.

Consider, where relevant:

- OWASP ASVS for application and API security verification
- OWASP Top 10 for common web application risks
- OWASP API Security Top 10 for API-specific risks
- OWASP SAMM for software security maturity across the SDLC
- NIST SSDF for secure software development practices
- SLSA for software supply-chain integrity
- SBOM practices such as CycloneDX or SPDX
- ISO/IEC 27001 for information security management
- ISO/IEC 27002 for security controls
- ISO/IEC 27701 for privacy information management
- ISO/IEC 27017 for cloud security controls
- ISO/IEC 27018 for protection of personal data in public clouds
- ISO/IEC 42001 where AI management system controls are relevant
- EU AI Act where the system is or incorporates an AI system deployed in the EU: assess prohibited practice risk, transparency obligations, high-risk classification, conformity assessment and GPAI model obligations where applicable
- NIST AI RMF (AI 100-1) where governance of AI risk is required outside the EU
- CCPA / CPRA where California consumer rights, opt-out of sale/sharing, or sensitive personal information controls are relevant
- SOC 2 where customer trust, SaaS assurance or vendor due diligence is relevant
- CIS Controls or CIS Benchmarks where infrastructure hardening is relevant
- NIST Cybersecurity Framework where enterprise security governance is relevant
- PCI DSS where payment card data is processed
- HIPAA or local health-data regulations where health data is processed
- eIDAS, PSD2, DORA, NIS2 or sector-specific regulations where relevant
- Internal policies, customer security requirements or procurement requirements

For each recommended framework or standard, state:

- Why it applies
- Scope of application
- Whether it is mandatory, strongly recommended or optional
- What evidence is already present
- What evidence is missing
- Which CI/CD checks or controls support it
- Whether specialist compliance/legal review is required

Do not recommend frameworks merely because they are popular. Tie each recommendation to the actual system and risk profile.

## Step 4: Security control review

Review security controls across the codebase.

Assess:

- Authentication
- Authorisation
- Session management
- Token lifecycle
- Credential storage
- Secret management
- Input validation
- Output encoding
- Injection risks
- File upload handling
- Path traversal risks
- SSRF risks
- CSRF risks where relevant
- XSS risks where relevant
- Deserialisation risks
- Command execution risks
- Cryptography use
- Randomness
- TLS and certificate validation
- API security
- Rate limiting
- Abuse prevention
- Bot protection where relevant
- Multi-tenancy isolation
- Tenant data separation
- Audit logging
- Security logging
- Error handling
- Secure configuration
- Debug mode exposure
- Admin interfaces
- Webhook verification
- Background jobs
- Message queues
- AI prompt injection risks where relevant
- AI output disclosure: whether the user is informed that content is AI-generated where legally or contractually required
- GDPR Article 22 automated decision-making: whether AI outputs influence decisions about individuals without human review
- External service trust boundaries
- Export controls: whether cryptographic functionality used or bundled is subject to EAR (US Bureau of Industry and Security), EU dual-use regulations or equivalent controls in the distribution jurisdiction; flag if no export compliance assessment is present

For each security finding, identify the control gap, evidence, impact and required fix.

## Step 5: Privacy and data-protection control review

Review privacy and data-protection controls.

Assess:

- Data minimisation
- Purpose limitation
- Privacy notices or disclosure evidence
- Consent capture where relevant
- Data subject access support
- Correction support
- Deletion support
- Portability support
- Retention enforcement
- Data export controls
- Masking and redaction
- Pseudonymisation or anonymisation
- Personal data in logs
- Personal data in test data
- Personal data in analytics
- Personal data in error messages
- Personal data in AI prompts or model outputs
- Personal data in support exports
- Personal data in backups
- Cross-border transfer risks
- Subprocessor or third-party sharing
- Access controls for personal data
- Auditability of personal-data access
- Default privacy settings
- Privacy by design and by default

For each privacy finding, identify the data involved, risk, evidence and required remediation.

## Step 6: Dependency, CVE and supply-chain review

Review:

- Dependency manifests
- Lock files
- Package pinning
- Transitive dependency visibility
- Known CVE scan configuration
- Dependency update strategy
- Licence risk
- Unmaintained packages
- Vendored code
- Binary artefacts
- Container base images
- Build scripts
- CI runners
- Package registries
- SBOM generation
- Provenance or signing
- SLSA-relevant controls
- Secret exposure in build logs
- Dependency confusion risk
- Typosquatting risk
- Build reproducibility

If CVE evidence is unavailable, recommend the appropriate local and CI dependency scanning strategy for the detected stack.

## Step 7: Infrastructure and deployment security review

Where infrastructure or deployment artefacts are present, assess:

- Least privilege IAM
- Network segmentation
- Public exposure
- Firewall/security group rules
- Secrets in IaC
- Encryption at rest
- Encryption in transit
- Backup protection
- Logging and monitoring
- Key management
- Environment separation
- Production access controls
- Admin access controls
- Remote access
- Container hardening
- Image scanning
- Runtime security
- Kubernetes security where relevant
- Serverless security where relevant
- Cloud security posture checks where relevant

Mark missing infrastructure evidence as "Not verifiable" rather than assuming safety.

## Step 8: Security and privacy test assessment

Assess whether security and privacy behaviours are tested.

Check for tests covering:

- Unauthenticated access
- Unauthorised access
- Role/permission boundaries
- Tenant isolation
- Input validation
- Injection attempts
- File upload restrictions
- Path traversal
- SSRF
- CSRF where relevant
- XSS where relevant
- Rate limits
- Token expiry
- Token revocation
- Password reset or account recovery where relevant
- Webhook signature validation
- Sensitive data redaction
- Logging exclusions
- Deletion and retention
- Data export
- Privacy defaults
- Error handling
- Security headers where relevant
- Dependency or supply-chain gates in CI

Flag weak tests that merely assert success, mock away the security boundary, avoid negative cases, or duplicate the implementation.

## Step 9: CI/CD security and compliance strategy

Recommend a CI/CD strategy based on the detected stack, risk profile and applicable frameworks.

Classify checks as:

- Required before every merge
- Required before release
- Scheduled daily or weekly
- Manual or periodic review
- Optional improvement

Consider:

- Formatting and linting
- Type checking or static analysis
- Unit tests
- Integration tests
- Security-focused tests
- Privacy-focused tests
- SAST
- SCA/dependency scanning
- CVE scanning
- Secret scanning
- Container image scanning
- IaC scanning
- SBOM generation
- Licence scanning
- DAST where applicable
- API security testing
- Fuzzing where appropriate
- Supply-chain provenance/signing
- Build reproducibility
- Artifact signing
- Policy-as-code
- Compliance evidence generation
- Test coverage
- Security regression tests
- Privacy regression tests
- Manual approval gates for high-risk changes

For each recommended CI gate, state:

- Purpose
- Tool category
- Stack-specific examples, if known
- Frequency
- Blocking or non-blocking
- Failure threshold
- Evidence produced
- Framework or compliance mapping

Do not require every possible tool. Recommend a practical staged strategy.

## Step 10: Native and platform-specific security

If the system is a native desktop application, mobile application, embedded system or firmware, assess the following in addition to the general controls in Step 4.

### Apple platform (macOS, iOS, iPadOS, tvOS, visionOS)

- App sandbox entitlements: are entitlements minimal and justified? Are any over-broad entitlements present (e.g. `com.apple.security.files.all`)?
- Hardened Runtime: is Hardened Runtime enabled? Are any exceptions enabled and justified?
- Code signing: is the binary signed with a Developer ID or App Store distribution certificate? Is the signing identity correct for the distribution channel?
- Notarisation: has the binary been notarised with Apple's notary service? Is the ticket stapled?
- Gatekeeper bypass risk: does any update or plugin loading mechanism load unsigned or unnotarised code?
- Keychain use: are secrets stored in the Keychain with appropriate accessibility constants? Are items protected with `kSecAttrAccessibleWhenUnlockedThisDeviceOnly` or stricter where appropriate?
- App Transport Security: is ATS enabled? Are any ATS exceptions present and justified?
- Privacy manifest (PrivacyInfo.xcprivacy): is a privacy manifest present and complete? Does it accurately declare API use, data collection and tracking?
- System extension and kernel extension risk: are any system or kernel extensions present? Are they signed and notarised?
- Inter-process communication: are XPC services used? Are entitlement checks present on the XPC listener?
- URL scheme handling: are custom URL schemes registered? Is input from URL schemes validated before use?
- Clipboard access: does the app access the clipboard? Is access limited to user-initiated events?
- iCloud and CloudKit: if used, is data encrypted before being stored in iCloud? Are sync conflicts handled safely?

### Android

- Permissions: are permissions minimal and justified? Are dangerous permissions explained to the user at the point of use?
- Intent handling: are exported components (activities, services, receivers) protected with appropriate permissions?
- WebView security: is JavaScript enabled only where necessary? Is `setAllowFileAccess` or `setAllowUniversalAccessFromFileURLs` enabled? Is input to `loadUrl` validated?
- Certificate pinning: is certificate pinning implemented for sensitive API calls?
- ProGuard / R8: is code obfuscation applied to release builds?
- Backup flag: is `android:allowBackup` set to false for sensitive apps?

### Windows

- Code signing: is the binary Authenticode-signed?
- Installer security: is the installer signed? Does it request only necessary privileges?
- Registry and file permission hardening: does the app write to user-writable locations only?

### Cross-platform native apps

- Auto-update mechanism: is the update delivery channel authenticated and integrity-verified? Can a downgrade attack be performed?
- Plugin or extension loading: is dynamically loaded code signed and verified before execution?
- Embedded web content: if a WebView or embedded browser is used, is the content security policy enforced? Is navigation to external origins restricted?

For each finding, identify the platform, control gap, evidence type, risk and recommended fix.

## Step 11: Incident response and vulnerability disclosure

Assess whether the organisation has defined and implemented controls for detecting, responding to, and disclosing security incidents and vulnerabilities.

### Incident response readiness

- Is a written incident response plan present?
- Does the plan define roles, responsibilities and escalation paths?
- Are GDPR breach notification timelines defined (72-hour supervisory authority notification, without-undue-delay data-subject notification)?
- Are equivalent breach notification timelines defined for non-EU jurisdictions where relevant (e.g. US state breach notification laws, which vary from 30 to 90 days)?
- Is there a defined process for forensic evidence preservation?
- Have incident response procedures been tested (tabletop exercise, simulation)?
- Is there a defined process for post-incident review and remediation tracking?

### Vulnerability disclosure

- Is a coordinated vulnerability disclosure (CVD) or responsible disclosure policy published (e.g. as a SECURITY.md file or at a known URL such as /.well-known/security.txt)?
- Is a bug bounty programme present or planned?
- Is there a defined process for triaging and remediating reported vulnerabilities?
- Is there a defined SLA for acknowledging and resolving reported vulnerabilities?

### Penetration testing

- Has a penetration test been conducted by an independent third party?
- What was the scope and date of the most recent test?
- Were critical and high findings from the most recent test remediated?
- Is penetration testing required by a customer contract, certification (SOC 2, ISO 27001) or regulatory obligation?
- Is penetration testing scheduled at a defined cadence (e.g. annually, after major releases)?

For each gap, classify the evidence type, risk and recommended action.

## Step 12: Risk-based compliance mapping

Map findings and recommended controls to applicable frameworks.

Use this format:

| Framework / standard | Applicability | Relevant controls or areas | Evidence present | Evidence missing | Recommended action |
|---|---|---|---|---|---|

Include only frameworks that are relevant or explicitly required.

## Step 13: Findings and remediation plan

For each finding include:

- ID
- Severity
- Evidence type
- Area: Security / Privacy / GDPR / Dependency / CVE / Supply chain / Infrastructure / CI/CD / Compliance / Testing / Documentation
- Framework mapping, if relevant
- Evidence
- Why it matters
- Recommended fix
- Verification method
- CI/CD gate recommendation
- AI code-assist fix prompt

AI code-assist fix prompts must be specific, bounded and safe. They must instruct the coding assistant not to weaken existing controls, not to remove tests, not to suppress scanner findings without justification, and not to make broad unrelated rewrites.

## Required report format

Return the review using this structure:

# Security, Privacy, GDPR and Compliance Codebase Review Report

## 1. Executive judgement

Use one of:

- PASS
- PASS WITH MINOR ISSUES
- CONDITIONAL PASS
- FAIL
- NOT VERIFIABLE

Explain the judgement in 3 to 6 sentences.

Clearly distinguish:

- Security readiness
- Privacy readiness
- GDPR readiness
- Dependency/CVE readiness
- Supply-chain readiness
- CI/CD security readiness
- Evidence completeness

## 2. System and data profile

Include:

- System type
- Stack and runtime
- Deployment model
- User groups
- Authentication model
- Authorisation model
- Data stores
- External services
- Personal data handled
- Sensitive data handled
- Regulated data handled
- Secrets used
- Logging and telemetry
- Existing security tooling
- Existing privacy controls
- Evidence supplied
- Evidence missing

## 3. Clarifications requested and answers received

Include:

- Question asked
- Why it mattered
- User answer, if provided
- Impact on the review
- Remaining uncertainty, if any

If no clarification was required, state:

"No clarification was required based on the supplied material."

## 4. GDPR and global data-protection assessment

Include:

- Likely GDPR applicability
- Controller/processor uncertainty
- Personal data categories
- Sensitive data categories
- Lawful basis evidence
- Transparency/privacy notice evidence
- Data minimisation assessment
- Retention and deletion assessment
- Data subject rights assessment
- Cross-border transfer assessment
- DPIA requirement assessment
- Processor/subprocessor concerns
- Breach readiness concerns
- Non-EU privacy laws triggered (CCPA/CPRA, LGPD, PIPEDA, Australian Privacy Act, others): for each, state whether triggered, key gap and required action
- Required actions

## 5. Recommended frameworks and standards

Use this table:

| Framework / standard | Recommended level | Why it applies | Scope | Evidence present | Evidence missing | Action |
|---|---|---|---|---|---|---|

Recommended level must be:

- Mandatory
- Strongly recommended
- Recommended
- Optional
- Not applicable

## 6. Security findings

For each finding include:

- ID
- Severity
- Evidence type
- Control area
- Evidence
- Risk
- Recommended fix
- Verification method
- AI code-assist fix prompt

## 7. Privacy findings

For each finding include:

- ID
- Severity
- Evidence type
- Data involved
- Evidence
- Risk
- Recommended fix
- Verification method
- AI code-assist fix prompt

## 8. Dependency, CVE and supply-chain findings

Include:

- Dependency risks
- CVE scan status
- Lock-file status
- SBOM status
- Licence risks
- Build provenance risks
- Container/base image risks where relevant
- Required actions
- AI code-assist fix prompts

## 9. Infrastructure and deployment security findings

Include findings where infrastructure evidence exists. If not available, state what is not verifiable.

## 10. Native and platform-specific security findings

Include findings from Step 10. If the system is not a native or platform-specific application, state "Not applicable".

For applicable platforms include:
- Code signing and notarisation status
- Entitlement and permission findings
- Hardened Runtime or equivalent status
- Embedded web content security
- Auto-update and plugin-loading security
- Any platform-specific findings

## 11. Incident response, vulnerability disclosure and penetration testing findings

Include:
- Incident response plan: present / absent / not verifiable
- Breach notification readiness: adequate / gap / not verifiable
- Coordinated vulnerability disclosure policy: present / absent / not verifiable
- Bug bounty programme: present / absent / not applicable
- Most recent penetration test: date, scope, critical/high findings remediated: yes / no / not verifiable
- Required actions

## 12. Security and privacy test assessment

Include:

- Security test coverage: Good / Adequate / Weak / Missing / Not verifiable
- Privacy test coverage: Good / Adequate / Weak / Missing / Not verifiable
- Meaningful tests found
- Weak or misleading tests found
- Missing tests
- Required test improvements

## 13. CI/CD security and compliance strategy

Use this table:

| Gate | Stage | Blocking | Frequency | Purpose | Evidence produced | Framework mapping |
|---|---|---:|---|---|---|---|
| Secret scanning |  |  |  |  |  |  |
| SAST |  |  |  |  |  |  |
| Dependency/CVE scan |  |  |  |  |  |  |
| Licence scan |  |  |  |  |  |  |
| SBOM generation |  |  |  |  |  |  |
| Container scan |  |  |  |  |  |  |
| IaC scan |  |  |  |  |  |  |
| Security tests |  |  |  |  |  |  |
| Privacy tests |  |  |  |  |  |  |
| DAST/API security test |  |  |  |  |  |  |
| Artifact signing/provenance |  |  |  |  |  |  |
| Platform security gate (code signing, notarisation, entitlement check) |  |  |  |  |  |  |
| Penetration test gate (block release if open critical/high findings exist) |  |  |  |  |  |  |
| Manual approval gate |  |  |  |  |  |  |

## 14. Risk-based compliance mapping

Use this table:

| Finding/control | GDPR / non-EU privacy | OWASP | NIST SSDF / AI RMF | EU AI Act | ISO/SOC/CIS/Other | Evidence | Required action |
|---|---|---|---|---|---|---|---|

## 15. Recommended action plan

Group actions as:

### Must fix before merge

### Must fix before release

### Must fix before external/customer deployment

### Should fix soon

### Evidence or documentation needed

Each action must be specific and testable.

## 16. AI code-assist prompt pack

Collect all fix prompts into a copy-paste-ready section.

Group prompts by:

- Authentication and authorisation
- Input validation and injection
- Secrets management
- Logging and data leakage
- Privacy and GDPR
- Non-EU privacy law compliance
- AI governance and EU AI Act
- Export controls
- Platform and native app security
- Incident response and vulnerability disclosure
- Dependency and CVE management
- Supply-chain security
- Infrastructure security
- Security tests
- Privacy tests
- CI/CD security gates
- Documentation and evidence

Each prompt must include:

- Scope
- Files or areas to inspect
- Evidence from the report
- Required change
- Tests to add or update
```
