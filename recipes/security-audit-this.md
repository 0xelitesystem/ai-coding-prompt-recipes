# security-audit-this

## When to use

Quick security review of AI-generated code (or any code). Not a substitute for professional pen-testing; a useful first pass.

## What to provide

- The code
- The trust boundary it sits on (handles user input? touches secrets? makes external calls?)
- Stack-specific context (web app? CLI? worker?)

## The prompt

```
Security review of this code.

Context:
- Trust boundary: [USER-FACING / INTERNAL / WORKER / etc.]
- Stack: [LANGUAGES, FRAMEWORK]
- Touches: [DATABASE / FILE SYSTEM / NETWORK / SECRETS / NONE]

Check for:
1. Input validation (every input that crosses a trust boundary)
2. Injection: SQL, NoSQL, command, path, HTML/XSS, regex DoS, prototype
   pollution, deserialization
3. Authentication: missing, weak, bypassable
4. Authorization: missing, broken (IDOR), privilege escalation
5. Secrets: hardcoded, logged, returned in errors
6. Data exposure: PII in logs/errors, missing redaction
7. Cryptography: weak algos, ECB mode, hardcoded IVs, custom crypto
8. Dependencies: known vulns (CVEs you know about)
9. Rate limiting / abuse: missing on auth/expensive endpoints
10. Error handling: stack traces returned to users, info disclosure

Output:
- High severity (exploitable now)
- Medium severity (defense in depth)
- Low severity (best practice)
- Out of scope (issues you can't determine from this code alone)

For each finding: file:line, what's wrong, what an attacker could do,
how to fix.
```

## What good output looks like

- Findings reference specific lines and explain the attack
- Severity is calibrated (not everything is High)
- "Out of scope" section acknowledges the agent's limits
- Includes both "this code is wrong" and "this code is missing X"

## Common failure modes

- **Hallucinates vulnerabilities.** Says line 42 has SQL injection when line 42 is using a parameterized query. Always verify each finding against the actual code.
- **Misses real issues.** False negatives are worse than false positives. The fix: pair this with a real human security review for anything serious.
- **Severity inflation.** Everything is "Critical." Recalibrate by asking: "if you had to pick the single most important issue, what is it?"
- **Generic warnings.** "This code might have security issues" with nothing specific. Push back: "what specifically?"

## Variations

**For auth-related code specifically:**
Replace check list with auth-specific checks: session management, password storage, token validation, refresh logic, MFA bypass paths, account recovery flows, rate limits on auth endpoints.

**For dependency review:**
Different recipe: "Review the dependency list (package.json / pyproject.toml / etc.). For each direct dependency: is it actively maintained, has it had recent CVEs, is the version pinned, are there known better alternatives. Skip transitive deps."

## Important caveat

This is a starting point, not a complete audit. Real security work involves threat modeling, architecture review, dynamic testing, and people who do this for a living. Don't ship security-critical code based on AI review alone.
