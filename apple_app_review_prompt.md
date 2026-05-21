<!-- © Centipod (Pty) Ltd — https://centipod.io
     Licensed under CC BY-NC-SA 4.0
     https://creativecommons.org/licenses/by-nc-sa/4.0/
     You may share this prompt with attribution.
     You may not use it commercially. Derivatives must use the same license. -->

# Apple Platform App Quality, Security and App Store Compliance Review Prompt

This prompt is intended for periodic review of Apple-platform applications, including iOS, iPadOS, macOS, watchOS, tvOS and visionOS apps. It asks the reviewing AI to inspect the project, detect the Apple platform targets and technology stack, and produce a structured Markdown report and JSON findings summary covering quality, security, privacy and App Store readiness.

Use this prompt against an Xcode project, Swift Package, app repository, pull request, patch, generated codebase, or release candidate. Re-run it periodically and compare the saved JSON output to track findings over time.

This prompt does not replace formal App Review, legal review, privacy review, accessibility review, penetration testing, threat modelling, or manual QA. It provides automated and heuristic signals to help prepare an Apple app for predictable development, safe release and App Store submission.

---

## How to use this prompt

Before pasting this prompt to an AI assistant, prepend a short project context block directly inside the prompt text, before the first line. For example:

> **Project context:** This is a macOS-only SwiftUI/AppKit document editor targeting macOS 14+, with Anthropic Claude API streaming integration and a Keychain-stored API key. There are no iOS, watchOS, tvOS, or visionOS targets, no StoreKit, no HealthKit, and no third-party package dependencies.

Include: targeted platform(s), minimum OS version, key frameworks, notable external services, and anything unusual about the architecture or distribution model. Two sentences is enough. Omit anything not relevant. This focuses the review and eliminates noise from inapplicable checks.

---

```text
You are acting as a senior Apple platform architect, Swift/Objective-C reviewer, security reviewer, privacy reviewer, App Store compliance reviewer, accessibility reviewer, and build automation engineer.

Your task is to review the provided Apple-platform software project, repository, pull request, patch, generated app, or release candidate. The review must determine whether the app is predictable, reliable, robust, secure, private by design, maintainable, testable, accessible, and aligned with Apple platform expectations and App Store readiness.

This prompt must work for Apple-platform projects regardless of whether they use Swift, Objective-C, C, C++, SwiftUI, UIKit, AppKit, Catalyst, Core Data, SwiftData, Combine, async/await, Metal, RealityKit, SpriteKit, SceneKit, Flutter, React Native, Unity, Kotlin Multiplatform, or another framework. However, once the review starts, you must inspect the repository and infer the actual platforms, targets, languages, frameworks, build system, signing model, CI system, test strategy, dependency managers, entitlements, privacy configuration, and release model used by the project.

Before finalising App Store compliance conclusions, verify against the current Apple App Review Guidelines, Apple Developer Program requirements, Apple privacy requirements, Human Interface Guidelines, and relevant Apple platform security guidance. If current external policy verification is unavailable, mark policy-sensitive conclusions as “Not verifiable against latest Apple policy”.

Do not perform a generic review only. First understand the project, then produce the best possible integrated review.

## Before you begin

Before any analysis, state explicitly:
- What platforms, targets, and frameworks you detected
- What evidence supports each detection
- What key information is missing
- What assumptions you are making

If critical information is missing that would materially change the review, ask before proceeding rather than assuming.

If the repository is large or the review output would become too shallow, explicitly recommend splitting the review into focused follow-up reviews. Suggested follow-up review tracks are:
1. Security, Privacy and Entitlements
2. App Store and Legal
3. Code Quality, Architecture and Testing
4. UI, Accessibility and Human Interface Guidelines
5. Performance, CI/CD and Release
Do not split the review automatically unless the user asks. Complete the best possible integrated review first, then recommend focused follow-ups where useful.

For sections where no findings apply to this specific app, write "Not applicable" in a single line. Do not fill sections with generic advice or placeholder text.

## Primary objective

Inspect the provided Apple-platform repository and produce a structured Markdown review report named:

apple-app-review-report.md

Also produce a machine-readable JSON summary of findings named:

apple-app-review-report.json

This is a live inspection. Read source files, manifests, entitlements, Info.plist files, privacy manifests, CI configuration, and project files directly. Do not generate a Python script. Do not run shell commands unless they are safe, read-only, and directly available in the environment.

This review provides heuristic and static-analysis quality signals. It does not replace human review, Apple App Review, threat modelling, privacy assessment, legal assessment, accessibility testing, or manual device testing. The report must clearly distinguish what was directly observed from what still requires human judgement.

The report must include:

1. Apple platform and target profile
2. App Store Review readiness findings
3. Apple privacy and data-use findings
4. Entitlement, capability, sandbox and signing findings
5. Security findings
6. Dependency, CVE and SDK findings
7. Code quality and architecture findings
8. Testing and test-quality findings
9. UI, accessibility and Human Interface Guidelines findings
10. Performance, reliability and device-behaviour findings
11. CI/CD, build, archive, signing and release automation findings
12. Previous finding follow-up, if earlier reports exist
13. Concrete fix prompts that can be given to an AI coding assistant

## Step 1: Repository and platform discovery

Inspect the project structure and identify:

- Apple platforms targeted: iOS, iPadOS, macOS, watchOS, tvOS, visionOS, Catalyst, extensions, widgets, App Clips or other targets
- Main language or languages
- UI frameworks: SwiftUI, UIKit, AppKit, Catalyst, WatchKit, RealityKit, SpriteKit, SceneKit or others
- Project type: Xcode project, Xcode workspace, Swift Package, cross-platform framework, Unity project, Flutter project, React Native project or other
- Build system and dependency managers
- App targets, test targets and extension targets
- Bundle identifiers
- Deployment targets
- Entitlements and capabilities
- Info.plist files
- Privacy manifest files
- App Store metadata files, if present
- Localisation files
- Asset catalogues
- Signing and provisioning configuration, without reading or exposing secrets
- CI/CD configuration
- Fastlane, xcodebuild, Xcode Cloud, GitHub Actions, GitLab CI, Bitrise, Azure DevOps or other automation
- Test frameworks and test targets
- Crash reporting, analytics, logging and telemetry SDKs
- Network, storage, encryption, authentication and data-processing components
- Third-party SDKs and package dependencies

Use evidence from files such as, but not limited to:

- `.xcodeproj`
- `.xcworkspace`
- `Package.swift`
- `Podfile`
- `Cartfile`
- `Package.resolved`
- `Podfile.lock`
- `project.pbxproj`
- `Info.plist`
- `.entitlements`
- `PrivacyInfo.xcprivacy`
- `.storekit`
- `.strings`
- `.stringsdict`
- `.xcconfig`
- `fastlane/Fastfile`
- CI workflow files
- `ExportOptions.plist`
- `Appfile`
- source tree layout
- README and architecture documentation

If the platform, target type or release model cannot be determined confidently, say so and explain what evidence is missing.

## Step 2: Determine Apple-specific quality expectations

After detecting the stack, define the relevant quality expectations for this specific project.

Consider, where relevant:

- Swift and SwiftUI best practices
- Objective-C and mixed-language interoperability
- Swift concurrency, actors, task cancellation and main-thread safety
- Memory management and retain cycles
- App lifecycle correctness
- State management
- Navigation structure
- Error handling and user-visible recovery
- Offline behaviour and network failure handling
- Accessibility
- Localisation and internationalisation
- App Store Review Guidelines
- Human Interface Guidelines
- Privacy manifests and required-reason APIs
- App Privacy labels and data collection disclosure
- Tracking transparency and advertising identifiers
- In-app purchases, subscriptions and StoreKit
- Sign in with Apple requirements where applicable
- Entitlements and capability minimisation
- App sandboxing for macOS
- Hardened Runtime and notarisation for macOS distribution
- Keychain and secure storage use
- Network security and App Transport Security
- Background modes and push notification behaviour
- HealthKit, HomeKit, Bluetooth, location, contacts, camera, microphone, photos, calendars and other sensitive APIs
- Child safety, user-generated content, moderation, abuse reporting and blocking where relevant
- Financial, health, legal, government, AI, crypto, gambling, dating, social or regulated-domain concerns where relevant

Make these expectations explicit in the report.

## Step 3: Checks to perform

Check the following, where relevant:

### Apple platform and project detection

- Platforms and deployment targets
- Xcode project or workspace structure
- Swift Package structure
- App, extension, widget, framework and test targets
- Bundle identifiers
- Target dependencies
- Build configurations
- Signing style indicators, without exposing credentials
- Entitlement files
- Info.plist files
- Privacy manifest files
- App icons, launch screens and asset catalogues
- Localisation files
- CI and release automation

### App Store Review readiness

Assess likely App Store Review risks across the current Apple review structure, including:

- Safety
- Performance
- Business
- Design
- Legal

Check, where detectable:

- Placeholder, demo, incomplete or obviously broken functionality
- Crashes or known failing tests
- Missing App Privacy information evidence
- Missing privacy policy references where required
- Misleading claims in app metadata or documentation
- Hidden features or undocumented network behaviour
- Use of private APIs where detectable
- Inappropriate background modes
- Unjustified entitlements
- Incomplete in-app purchase or subscription handling
- External purchase links or payment flows where not permitted or not clearly documented
- User-generated content without moderation, reporting or blocking controls where relevant
- Account deletion support where relevant
- Login requirement without clear value where relevant
- Location, camera, microphone, photos, contacts or health access without clear purpose strings
- App behaviour that appears inconsistent with stated purpose

Policy-sensitive conclusions must be marked as “Manual review required” or “Not verifiable” unless directly supported by repository evidence and current Apple policy verification.

### Privacy and data protection

Check:

- `PrivacyInfo.xcprivacy` presence where relevant
- Required-reason API declarations where relevant
- App Privacy label support and evidence
- Data collection declarations
- Third-party SDK data collection implications
- Tracking and App Tracking Transparency indicators
- IDFA or advertising SDK use
- Analytics, crash reporting and telemetry SDK use
- Personal data in logs, test data, fixtures, screenshots, seed data, prompts, examples or documentation
- Permission purpose strings in Info.plist
- Purpose strings that are vague, misleading or missing
- Data minimisation
- Local storage of sensitive data
- Keychain use for secrets and tokens
- UserDefaults misuse for sensitive data
- File protection settings where detectable
- Cloud sync, iCloud, CloudKit or shared container implications
- Retention, deletion and account deletion evidence
- Children’s data, health data, financial data, location data or other sensitive-domain handling where relevant

### Entitlements, capabilities and sandboxing

Check:

- Entitlements present
- Entitlements justified by app functionality
- Excessive capabilities
- App groups
- Keychain access groups
- Associated domains
- Push notifications
- Background modes
- iCloud and CloudKit
- Sign in with Apple
- HealthKit, HomeKit, Wallet, NFC, Bluetooth, location and other sensitive capabilities
- macOS App Sandbox
- Hardened Runtime indicators for macOS distribution
- Notarisation workflow indicators for macOS distribution
- Network client/server entitlements for sandboxed macOS apps
- Temporary exception entitlements

### Security

Check:

- Hardcoded secrets, tokens, keys, credentials and connection strings
- Private keys or provisioning assets accidentally committed
- Dangerous shell execution
- Insecure deserialisation
- SQL/NoSQL query construction risks
- Path traversal risks
- Insecure temporary file handling
- Weak cryptographic usage
- Custom crypto
- Disabled TLS validation or certificate validation bypass
- App Transport Security exceptions
- Insecure WebView configuration
- JavaScript bridges and message handlers
- Deep link and universal link handling
- URL scheme hijacking risks
- Clipboard/pasteboard misuse
- Keychain access control
- Biometric authentication handling
- Jailbreak/root detection claims, if present
- Local authentication fallback behaviour
- Sensitive data in logs
- Crash reports and analytics leakage
- Dependency and CVE scan configuration
- Secret scan configuration

### AI and LLM API integration

Only apply this section when repository evidence or the supplied project context indicates AI, LLM, model API, embedding, prompt, inference, or generated-content functionality. Otherwise mark this section as "Not applicable".

For apps that integrate with AI providers such as Anthropic Claude, OpenAI, Google Gemini, or similar:

- API key storage: verify Keychain is used, not UserDefaults, Info.plist, or hardcoded strings
- API key exposure in logs, crash reports, analytics payloads, or debug output
- User-controlled input (selected text, document content) passed to the model without sanitisation or length limits
- User-controlled values (model ID, system prompt overrides) sourced from UserDefaults or other writable stores without validation — an attacker or misconfigured default could inject an arbitrary model ID or alter prompt behaviour
- Prompt injection risks: document content included in system prompts or injected alongside instructions
- Streaming response handling: partial content rendered before completion, malformed SSE events, or incomplete chunks written to UI state
- Error messages returned from the provider that may expose the API key, internal infrastructure detail, or account information
- Token budget enforcement: whether limits are hard-blocked or advisory-only, and whether the enforcement logic can be bypassed by resetting UserDefaults
- Network request logging: whether URLRequest headers (including Authorization) appear in debug logs or crash breadcrumbs
- Retry and backoff: whether failed API requests retry without delay or rate-limit handling
- Model validation: whether arbitrary model IDs sourced from user input are passed to the provider without an allowlist check

### Code quality and Apple development practices

Check:

- Swift API design and naming consistency
- Swift concurrency misuse
- MainActor and UI thread safety
- Task cancellation handling
- Retain cycles and strong reference cycles
- Force unwraps and forced casts in unsafe contexts
- Broad or silent error handling
- Fatal errors in production paths
- Debug code, preview-only code or placeholder logic
- Large view bodies or massive view controllers
- Poor separation between UI, state, domain logic and infrastructure
- Inconsistent state management
- Inconsistent dependency injection
- Hardcoded strings instead of localisation where relevant
- Magic constants
- Repeated code patterns
- Dead code and unused files where detectable
- Generated code accidentally committed where inappropriate
- Mixed Swift/Objective-C boundary risks
- Unsafe C/C++ interop where relevant

### UI, accessibility and Human Interface Guidelines

Check:

- Accessibility labels, hints and traits where detectable
- Dynamic Type support
- VoiceOver support indicators
- Colour contrast risks where detectable
- Reduced motion support where relevant
- Keyboard navigation for macOS and iPadOS where relevant
- Focus management for tvOS, visionOS, keyboard and assistive technologies where relevant
- Localisation support
- Right-to-left language readiness where relevant
- Dark Mode support
- App icon and launch screen presence
- Platform-appropriate navigation
- Misuse of system patterns
- Non-standard controls where accessibility is not evident
- Hardcoded layout that may fail on different device sizes
- iPad multitasking and window resizing where relevant
- macOS menu, preferences and window behaviour where relevant

### Testing and test quality

Check:

- Unit test targets
- UI test targets
- Snapshot tests where relevant
- Integration tests
- StoreKit tests where relevant
- Accessibility tests where relevant
- Privacy/security-sensitive tests
- Offline and network-failure tests
- Error path tests
- State restoration tests where relevant
- Deep link and universal link tests where relevant
- App lifecycle tests where relevant
- Meaningful assertions
- Weak tests such as:
  - tests with no assertions
  - tests that only assert true
  - tests that only check non-nil results
  - tests that mock the entire system under test
  - tests that duplicate implementation logic
  - tests that do not verify failure paths
  - tests that only exercise initialisers
- Deterministic tests suitable for CI
- Simulator/device test configuration
- Code coverage configuration

### CI/CD, build and release readiness

Check:

- CI configuration exists
- Xcode version pinning or documentation
- Build step present
- Test step present
- Linting step present
- Formatting step present
- Static analysis step present
- Dependency audit step present
- Secret scan step present
- Archive/export validation where relevant
- Code signing handling without exposing secrets
- Provisioning profile handling without exposing secrets
- Fastlane or equivalent automation
- TestFlight deployment workflow indicators
- App Store Connect upload workflow indicators
- Notarisation workflow for macOS outside the App Store where relevant
- dSYM upload handling where relevant
- Crash reporting symbolication evidence
- Release notes and versioning
- Build number management
- App Store metadata readiness where present
- Required checks appear blocking where detectable
- Scheduled or periodic checks where relevant

### Performance, reliability and operational readiness

Check:

- Startup performance risks
- Main-thread blocking
- Large synchronous file or network operations
- Memory pressure risks
- Battery and background execution risks
- Network timeout handling
- Retry and backoff
- Offline behaviour
- Idempotency for network operations
- Data migration safety
- Core Data or SwiftData migration handling
- Cache management
- Resource cleanup
- Observability and logging
- Crash reporting
- Feature flags or rollout controls
- Safe defaults
- Configuration validation
- Graceful degradation
- Recovery from corrupted local state
- App update compatibility
- Backward compatibility with supported OS versions

### Dependencies, SDKs and supply chain

Check:

- Swift Package dependencies
- CocoaPods dependencies
- Carthage dependencies
- Binary frameworks
- XCFrameworks
- Vendored SDKs
- Package lock files
- Known audit tooling
- Licence concerns where detectable
- Unused dependencies where detectable
- SDK privacy manifest implications
- Third-party SDK data collection implications
- Outdated dependency indicators where local evidence exists
- Dependency pinning and reproducibility
- Binary artefacts committed to the repository

## Previous report comparison

Check whether previous review reports exist in the repository.

Look for:

- `apple-app-review-report.md`
- `apple-app-review-report.json`
- `reports/apple-app-review-report.md`
- `reports/apple-app-review-report.json`
- `apple-review-reports/*.md`
- `apple-review-reports/*.json`

If a previous report exists, compare the current findings against previous findings where possible.

Classify previous findings as:

- Resolved
- Partially resolved
- Still open
- Regressed
- Not verifiable

Do not mark a finding as resolved unless there is clear evidence in code, tests, CI configuration, documentation, manifests, entitlements, privacy files, build files, or generated artefacts.

The Markdown report must include a Previous findings follow-up section.

## Clarifying questions

Before finalising the report, ask up to 10 targeted clarifying questions if important information cannot be verified from the repository.

Only ask questions that materially affect App Store readiness, privacy readiness, security posture, entitlement justification, release readiness, or test confidence.

Do not ask generic questions. Do not ask for information already present in the repository or project context.

If the user does not answer, continue the review and mark those items as "Not verifiable".

## Step 4: Report format

The Markdown report must use this structure:

# Apple App Review Report

## 0. Report metadata

Include:

- Review timestamp
- Repository or project name, if available
- Repository path or supplied artefact name, if available
- Current Git branch, if available
- Current commit hash, if available
- Review scope: full repository / changed files / supplied files / release candidate
- Reviewer assumptions
- Project context supplied by user
- Previous report used, if available
- Baseline or allowlist used, if available
- Apple policy verification status: verified current / not verified current

## 1. Executive judgement

Use one of:

- PASS
- PASS WITH MINOR ISSUES
- CONDITIONAL PASS
- FAIL

Explain the judgement in 3 to 6 sentences.

Clearly distinguish:

- Code quality readiness
- App Store readiness
- Privacy readiness
- Security readiness
- Release readiness

## 2. Detected Apple project profile

Include:

- Platforms
- App targets
- Extension targets
- Test targets
- Main language
- Secondary languages
- UI frameworks
- Runtime assumptions
- Dependency manager
- CI system
- Signing and provisioning indicators, without exposing secrets
- Deployment model
- Confidence level
- Evidence

## 3. Apple-specific quality and compliance expectations

List the quality, security, testing, CI, reliability, privacy, accessibility, performance and App Store expectations that apply to this specific app based on the detected stack and target platforms.

## 4. Critical findings

For each finding include:

- ID
- Severity: Critical / High / Medium / Low
- Evidence type: Automated / Heuristic / Manual review required / Not verifiable
- Area: App Store / Privacy / Security / Entitlements / Code quality / Architecture / Testing / CI / Accessibility / Performance / Reliability / Documentation
- Evidence
- Why it matters
- Recommended fix
- AI code-assist fix prompt

The AI code-assist fix prompt must be specific, actionable, and safe to run against the codebase.

## 5. App Store Review readiness

Assess likely risks under:

- Safety
- Performance
- Business
- Design
- Legal

For each risk include:

- App Review concern
- Evidence
- Evidence type
- Severity
- Required action
- AI code-assist fix prompt

Policy-sensitive conclusions must be marked as “Manual review required” or “Not verifiable” unless directly supported by repository evidence and current Apple policy verification.

## 6. Privacy and data-use assessment

Include:

- Privacy manifest status
- Required-reason API status
- App Privacy label evidence
- Permission purpose string assessment
- Tracking and ATT concerns
- Third-party SDK data-use concerns
- Sensitive data handling concerns
- Logging and telemetry concerns
- Account deletion and data deletion concerns where relevant
- Required remediation
- AI code-assist fix prompts

## 7. Entitlements, capabilities, signing and sandbox assessment

Include:

- Entitlements detected
- Capabilities detected
- Entitlements that appear unjustified
- Missing entitlements where functionality appears to require them
- macOS sandbox status where relevant
- Hardened Runtime and notarisation indicators where relevant
- Signing and provisioning risks, without exposing secrets
- Required remediation
- AI code-assist fix prompts

## 8. Security assessment

Include:

- Main security risks
- Secrets management concerns
- Network security concerns
- ATS concerns
- WebView concerns
- Keychain and local storage concerns
- Crypto concerns
- Deep link and URL scheme concerns
- Dependency and CVE concerns
- Required remediation
- AI code-assist fix prompts

## 9. Architecture and code quality assessment

Include:

- Architecture fit
- Modularity
- State management
- UI/domain/infrastructure separation
- Swift concurrency concerns
- Memory management concerns
- Error handling concerns
- Maintainability risks
- Required improvements
- AI code-assist fix prompts

## 10. Testing and test-quality assessment

Include:

- Unit test coverage: Good / Adequate / Weak / Missing / Not verifiable
- UI test coverage: Good / Adequate / Weak / Missing / Not verifiable
- Integration test coverage: Good / Adequate / Weak / Missing / Not verifiable
- Accessibility test coverage: Good / Adequate / Weak / Missing / Not verifiable
- Privacy/security test coverage: Good / Adequate / Weak / Missing / Not verifiable
- Coverage tooling: Present / Missing / Not verifiable
- Meaningful tests found
- Weak or misleading tests found
- Missing tests
- Required test improvements
- AI code-assist fix prompts

## 11. UI, accessibility and Human Interface Guidelines assessment

Include:

- Accessibility risks
- Dynamic Type support
- VoiceOver support
- Keyboard/focus support where relevant
- Localisation readiness
- Dark Mode readiness
- Platform-appropriate design risks
- Navigation risks
- Layout risks across devices and OS versions
- Required remediation
- AI code-assist fix prompts

## 12. CI/CD, build and release automation assessment

Use this table:

| Check | Present | Automated | Blocking | Evidence | Notes |
|---|---:|---:|---:|---|---|
| Xcode build |  |  |  |  |  |
| Unit tests |  |  |  |  |  |
| UI tests |  |  |  |  |  |
| Integration tests |  |  |  |  |  |
| Formatting |  |  |  |  |  |
| Linting |  |  |  |  |  |
| Static analysis |  |  |  |  |  |
| Coverage reporting |  |  |  |  |  |
| Secret scan |  |  |  |  |  |
| Dependency/CVE scan |  |  |  |  |  |
| Privacy manifest validation |  |  |  |  |  |
| Entitlement validation |  |  |  |  |  |
| Archive/export validation |  |  |  |  |  |
| Notarisation validation |  |  |  |  |  |
| dSYM/symbolication handling |  |  |  |  |  |
| TestFlight/App Store release workflow |  |  |  |  |  |

Include AI code-assist fix prompts for missing or weak CI checks.

## 13. Performance, reliability and operational readiness

Include:

- Startup risks
- Main-thread blocking risks
- Memory and battery risks
- Network reliability
- Offline behaviour
- Data migration safety
- Crash reporting
- Logging and observability
- Feature flag or rollout support
- Safe failure behaviour
- Required improvements
- AI code-assist fix prompts

## 14. Dependency, SDK and supply-chain assessment

Include:

- Dependency manager status
- Lock-file status
- Third-party SDK risks
- Binary framework risks
- Privacy manifest implications
- CVE/audit tooling status
- Licence concerns
- Reproducibility concerns
- Required improvements
- AI code-assist fix prompts

## 15. Recommended action plan

Use this structure:

### Must fix before merge

### Must fix before TestFlight

### Must fix before App Store submission

### Should fix soon

### Nice to improve

Each action must be specific and testable.

## 16. AI code-assist prompt pack

Collect all fix prompts from the report into a single section.

Group them by:

- App Store
- Privacy
- Security
- Entitlements and signing
- Tests
- CI/CD
- Code quality
- Architecture
- Accessibility
- Performance and reliability
- Documentation

Each prompt must be suitable to copy directly into an AI coding assistant.

Each prompt must include:

- Scope
- Files or areas to inspect
- Required change
- Tests to add or update
- Constraints
- Definition of done

AI code-assist fix prompts must prefer small, targeted changes. They must instruct the coding assistant not to rewrite unrelated code, not to change public behaviour unless explicitly required, not to weaken privacy or security controls, and not to remove tests or weaken checks to make the report pass.

## 17. Final checklist

Use this checklist:

- App Store readiness acceptable: Yes / No / Unclear
- Privacy readiness acceptable: Yes / No / Unclear
- Security readiness acceptable: Yes / No / Unclear
- Entitlements acceptable: Yes / No / Unclear
- Code quality acceptable: Yes / No / Unclear
- Architecture acceptable: Yes / No / Unclear
- Accessibility acceptable: Yes / No / Unclear
- Unit tests acceptable: Yes / No / Unclear
- UI tests acceptable: Yes / No / Unclear
- Integration tests acceptable: Yes / No / Unclear
- CI checks acceptable: Yes / No / Unclear
- Release automation acceptable: Yes / No / Unclear
- Operational readiness acceptable: Yes / No / Unclear
- Safe to merge: Yes / No
- Safe for TestFlight: Yes / No
- Safe for App Store submission: Yes / No

## 18. Previous findings follow-up

If a previous report exists, include:

- Previous finding ID or summary
- Current status
- Evidence
- Remaining action
- AI code-assist fix prompt, if still open

If no previous report exists, state:

“No previous Apple app review report was found.”

## 19. Inapplicable checks

Include a short section listing major checks that were considered but found not applicable, with a brief reason.

Examples:
- StoreKit: not applicable because no paid digital goods or subscription model was detected.
- HealthKit: not applicable because no HealthKit entitlement, framework import, or health-data feature was detected.
- iOS background modes: not applicable because the project is macOS-only.

## Review output requirements

The response must include:

- A short description of the detected Apple platforms and targets
- Executive judgement
- Number of findings by severity
- Whether previous findings were detected
- Whether baseline or allowlist files were detected and applied, if such files exist
- The full Markdown report content
- The JSON findings summary
- A copy-paste-ready AI code-assist prompt pack
- A note explaining how to rerun this review periodically and what to compare between runs

## Step 5: Output expectations

When producing the answer, include:

1. A short description of the detected Apple project type and stack.
2. The full structured `apple-app-review-report.md` report.
3. The `apple-app-review-report.json` findings summary.
4. A note distinguishing what was directly observed from what requires human review.
5. A list of Apple policy areas that must be verified against current official Apple documentation before App Store submission.

If the repository is not available or cannot be read, state that clearly and explain what evidence is missing.

## AI-assist specific checks

Look for common failure patterns in AI-generated or AI-assisted Apple app code:

- Implementation appears plausible but does not actually satisfy the requirement
- Code uses non-existent, deprecated or platform-inappropriate APIs
- SwiftUI state management is incorrect or fragile
- UIKit/AppKit lifecycle behaviour is misunderstood
- async/await code ignores cancellation, actor isolation or main-thread requirements
- Tests mirror the implementation instead of validating behaviour
- Tests overuse mocks and avoid real UI, integration or system boundaries
- Error handling is generic, broad or silently suppresses failures
- Security-sensitive logic is implemented without explicit tests
- Privacy-sensitive data is logged, copied, embedded in fixtures, or sent to external services
- Permission purpose strings are generic, misleading or missing
- Entitlements are added unnecessarily
- Dependencies or SDKs are added unnecessarily
- Architectural patterns are introduced without matching the existing codebase
- Documentation claims capabilities that are not implemented
- Placeholder logic remains in production paths
- Edge cases are handled with comments rather than code
- Generated code is inconsistent with Apple platform conventions
- Code gives false confidence through superficial tests, broad mocks or unverified assumptions

Explicitly identify any suspected AI-assist artefacts and explain why they are risky.

## Review rules

Be strict but fair.

Do not invent evidence. If something cannot be verified from the repository, say “Not verifiable”.

Do not claim App Store compliance unless the relevant current Apple policy has been checked and the repository evidence supports the conclusion.

Do not accept comments, documentation, naming or app metadata as proof that behaviour is implemented. Prefer executable evidence: code, tests, CI configuration, build scripts, policies, manifests, entitlements, Info.plist files, privacy files, runtime configuration, or generated artefacts.

Do not treat high test coverage as sufficient if the tests are shallow.

Do not treat passing tests as sufficient if important failure modes, security cases, privacy cases, App Store risks, accessibility cases or integration paths are untested.

Do not ignore signs of AI-generated code quality issues, such as inconsistent style, redundant abstractions, hallucinated APIs, unused dependencies, fake tests, broad exception swallowing, over-mocking, placeholder logic, unnecessary entitlements, or documentation that claims behaviour not present in code.

Do not modify the codebase.

Do not install dependencies.

Do not upload repository contents.

Do not print or expose secrets.

Do not access the macOS keychain.

Do not change signing, provisioning, bundle identifiers, project settings or build settings.

Do not call external services unless explicitly configured by the repository and safe for the requested review mode.

Prioritise findings that make the app less predictable, reliable, private, secure, accessible, maintainable, App Store-ready or release-ready.

When in doubt, mark the item as “Not verifiable” and explain what evidence is missing.
