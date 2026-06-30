# Module Request Template

Use this file when you want Claude to build a new feature, module, component, page, script, or service for this project without drifting away from existing patterns.

This is a user-facing prompt template.
Copy the template that fits your task, fill in the blanks, and paste it into the chat.

---

## Agent Normalization Rule

Even when you do not paste one of these templates, Claude should still route every task through this file internally before implementation.

- Classify the request using the closest template below
- Infer missing fields from the user request, the repository context, and the nearest existing reference files
- Identify dependent files, flows, scripts, and documentation that the requested change could affect
- Preserve existing dependent behavior unless the request explicitly changes it
- Ask follow-up questions only for gaps that would materially change the implementation
- Load the agent instruction files listed in the chosen template before coding
- After implementation, verify affected dependencies and report what was checked and whether it still works

---

## Agent Instruction Files

Always reference these explicitly in your prompt — do not rely on Claude to discover them automatically.

- `assets/agents/engineering-agent.md` — use for all implementation tasks
- `assets/agents/read-me-sync.md` — use when documentation must be updated
- `assets/agents/repo-audit-agent.md` — use for audits and pre-merge reviews

---

## Template 1: Full Feature (Frontend + Backend)

Use this when the feature needs both a frontend and a backend.

```text
Build a full feature for this project: <FEATURE_NAME>.

Read and follow these agent files explicitly:
- assets/agents/engineering-agent.md
- assets/agents/read-me-sync.md

This must follow existing project patterns exactly.
Do not introduce new patterns, folder structures, or abstractions.
Match the nearest existing reference files exactly.
Preserve dependent files, flows, and contracts that this feature touches unless the request explicitly changes them.

Work order:
1. Analyse nearest reference files (frontend and backend)
2. Identify dependent flows, pages, scripts, and documentation that must keep working
3. Build backend first
4. Build frontend against the real backend contract
5. Sync all affected documentation using the read-me-sync agent rules
6. Verify end to end, including affected dependencies, and report what was checked

Frontend reference file(s):
- <SRC/FRONTEND/REFERENCE_1>
- <SRC/FRONTEND/REFERENCE_2>

Backend reference file(s):
- <SCRIPTS_OR_BACKEND_REFERENCE_PATH>

Feature goal:
<WHAT THE FEATURE DOES>

Business rules:
- <RULE 1>
- <RULE 2>

Frontend requirements:
- <PAGE OR COMPONENT REQUIREMENTS>
- <INTERACTION REQUIREMENTS>
- <STATE / FEEDBACK REQUIREMENTS>

Backend requirements:
- <DATA MODEL OR SCHEMA>
- <ENDPOINT OR SCRIPT REQUIREMENTS>
- <VALIDATION REQUIREMENTS>

Important fields or data:
- <FIELD_NAME>: <TYPE / RULE>
- <FIELD_NAME>: <TYPE / RULE>

Done means:
- backend and frontend both implemented and connected
- loading, validation, and user feedback included
- no new patterns introduced
- all affected documentation updated
- dependent areas verified and the verification outcome reported
```

---

## Template 2: Frontend Only

Use this when the work is HTML/CSS/JS only and no backend changes are needed.

```text
Build a frontend feature for this project: <FEATURE_NAME>.

Read and follow these agent files explicitly:
- assets/agents/engineering-agent.md
- assets/agents/read-me-sync.md

Match the nearest existing frontend file exactly.
Do not introduce new patterns or third-party libraries without explanation.
If backend contract assumptions are needed, verify them before coding.
Preserve impacted pages, scripts, and styles unless the request explicitly changes them.

Reference file(s):
- <SRC/FRONTEND/REFERENCE_1>
- <SRC/FRONTEND/REFERENCE_2>

Feature goal:
<WHAT THE FRONTEND MUST DO>

Pages or components needed:
- <PAGE / SECTION / COMPONENT>

Behavior requirements:
- <INTERACTION RULES>
- <VALIDATION RULES>
- <FEEDBACK / STATE RULES>

Accessibility requirements:
- <ANY SPECIFIC A11Y REQUIREMENTS, OR "Follow WCAG 2.2 AA">

Dependent areas to verify and report:
- <IMPACTED PAGE OR FLOW>
- <IMPACTED SCRIPT OR STYLE>

Documentation:
- Update or create documentation files for all affected components
```

---

## Template 3: Small Change to an Existing File or Module

Use this for narrow updates to an existing feature, page, or script.

```text
Update the existing feature: <MODULE OR FILE NAME>.

Read and follow these agent files explicitly:
- assets/agents/engineering-agent.md

Do not refactor the module into a new pattern.
Stay inside the current structure of that exact file or module.
Identify dependent files, flows, and documentation that this change could affect and preserve them unless the request explicitly changes them.

Exact files or areas affected:
- <FILE OR FOLDER 1>
- <FILE OR FOLDER 2>

Change needed:
<DESCRIBE CHANGE CLEARLY>

Important existing behavior to preserve:
- <BEHAVIOR 1>
- <BEHAVIOR 2>

Dependent areas to verify and report:
- <DEPENDENT PAGE / FLOW / SCRIPT>
- <DEPENDENT CONTRACT OR DATA>
```

---

## Template 4: Documentation Sync

Use this when documentation has drifted and needs to be brought in line with the current codebase.

```text
Synchronize all repository documentation with the current state of the codebase.

Read and follow this agent file explicitly:
- assets/agents/read-me-sync.md

Scan the full repository structure.
Identify all outdated, missing, or inconsistent markdown files.
Update every affected documentation file so it reflects the current code accurately.

Known documentation gaps or triggers:
- <WHAT CHANGED THAT TRIGGERED THIS SYNC>
- <SPECIFIC FILES KNOWN TO BE OUTDATED>

Done means:
- README.md reflects current features, structure, and setup
- docs/SETUP.md has valid installation steps
- docs/USAGE.md matches current UI and features
- All folder READMEs match reality
- No broken references remain
- Commit message generated
```

---

## Template 5: Repository Audit

Use this before a merge, demo, or submission to get a full production-grade review.

```text
Perform a full repository audit for this project.

Read and follow this agent file explicitly:
- assets/agents/repo-audit-agent.md

Audit scope:
- Code quality
- Architecture
- Security
- Performance
- Accessibility (WCAG 2.2 AA)
- UI/UX
- Documentation
- Testing and reliability
- Dependencies
- Deployment readiness

Areas of particular concern:
- <ANY KNOWN WEAK AREAS OR RECENT CHANGES>

Done means:
- Full 14-section audit report produced
- Final Verdict given (Approve / Approve with fixes / Reject)
- Prioritized fix plan provided
- Commit message generated
```

---

## Recommended Default

If a feature needs both frontend and backend, use **Template 1** — describe the whole feature once and ask for backend first, then frontend.
That is better than two separate feature descriptions.

---

## Good Prompt Example

```text
Build a frontend feature for this project: User Login Page.

Read and follow these agent files explicitly:
- assets/agents/engineering-agent.md
- assets/agents/read-me-sync.md

Match the existing frontend structure in src/frontend exactly.
Do not introduce new libraries.
Preserve the existing index.html layout and style.css patterns.

Reference file(s):
- src/frontend/index.html
- src/frontend/css/style.css
- src/frontend/js/script.js

Feature goal:
Allow users to log in with their email and password. Show a friendly error message for invalid credentials. Redirect to index.html on success.

Pages or components needed:
- Login page (login.html already exists — update it)
- Form validation script

Behavior requirements:
- Validate email format on blur
- Disable submit button while loading
- Show inline error on bad credentials
- Redirect on success

Accessibility requirements:
- Follow WCAG 2.2 AA
- All inputs must have visible labels and focus states

Dependent areas to verify and report:
- src/frontend/login.html
- src/frontend/js/script.js
- Any nav links that point to login.html

Documentation:
- Update docs/USAGE.md to describe the login flow
```

---

## Important Notes

- Always reference agent instruction files explicitly in your prompt.
- If the work is full-stack, describe the whole feature once using Template 1.
- If the backend contract is not yet decided, say so clearly and ask for backend first.
- For audits before submission or demo, always use Template 5.
