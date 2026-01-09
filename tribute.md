# Tribute - Dependency Funding Discovery

Find verified funding links for the open source projects this repository depends on.

## Instructions

You will analyze this repository's dependencies and research verified funding links for them.

### Step 1: Read Dependency Files

Find and read the dependency files in this repository:

| File | How to Extract Dependencies |
|------|----------------------------|
| package.json | `dependencies` and `devDependencies` keys |
| requirements.txt | Package names (before `==`, `>=`, etc.) |
| pyproject.toml | `[project.dependencies]` or `[tool.poetry.dependencies]` |
| Cargo.toml | `[dependencies]` section |
| go.mod | `require` statements |
| Gemfile | `gem` statements |

Create a list of the direct dependencies (focus on `dependencies`, not `devDependencies` unless specifically relevant).

### Step 2: Research Funding Links

For the top 10 most important dependencies, research funding links using this exact process:

#### 2a. Find the GitHub Repository

For npm packages, the repo URL is usually in package.json under `repository`.
For Python packages, check PyPI or search for the package.
For others, search for "{package name} github".

#### 2b. Check FUNDING.yml (Most Reliable)

Use WebFetch to check if the repo has a FUNDING.yml:

```
https://raw.githubusercontent.com/{owner}/{repo}/main/.github/FUNDING.yml
```

If this returns content, parse it:
- `github: username` → Link is `https://github.com/sponsors/username`
- `open_collective: name` → Link is `https://opencollective.com/name`
- `patreon: name` → Link is `https://patreon.com/name`
- `ko_fi: name` → Link is `https://ko-fi.com/name`
- `custom: url` → Use the URL directly

#### 2c. Search for Funding (If No FUNDING.yml)

Use WebSearch:
```
"{package name}" github sponsors OR open collective OR funding official
```

Look for:
- Links to github.com/sponsors/{user}
- Links to opencollective.com/{project}
- Funding sections on official project websites

#### 2d. VERIFY THE LINK EXISTS (Critical!)

**Before including ANY funding link, you MUST verify it exists.**

Use WebFetch on the funding URL:
- If it returns a valid funding page → Include it
- If it returns 404 or "not found" or "not enrolled" → Do NOT include it

Example:
```
WebFetch: https://opencollective.com/webpack
→ Returns valid page with funding info → ✓ Include

WebFetch: https://github.com/sponsors/someuser
→ Returns "not enrolled in sponsors" → ✗ Do not include
```

### Step 3: Present Results

**Only show funding links you have verified.**

Format your response like this:

---

## Dependency Funding Report for {project}

### Summary
- Analyzed: {N} direct dependencies
- Verified funding found: {M} packages

### Verified Funding Links

| Package | Funding | How Verified |
|---------|---------|--------------|
| webpack | [Open Collective](https://opencollective.com/webpack) | FUNDING.yml |
| express | [GitHub Sponsors](https://github.com/sponsors/dougwilson) | FUNDING.yml |
| lodash | [Open Collective](https://opencollective.com/lodash) | Web search + verified |

### No Verified Funding Found

These packages don't have discoverable funding pages (or are corporate-maintained):
- react (Meta)
- typescript (Microsoft)
- {other packages}

### Quick Actions

To support these projects:
1. [Link to top verified funding page]
2. [Link to second funding page]
...

---

## Critical Rules

1. **NEVER present unverified links**
   - Every link must be checked with WebFetch before presenting
   - If verification fails, say "No verified funding"

2. **NEVER guess from training knowledge**
   - Don't assume `opencollective.com/{package}` exists without checking
   - Don't assume `github.com/sponsors/{user}` exists without checking
   - Your training data about funding links may be outdated

3. **Be transparent about verification**
   - Show how each link was verified (FUNDING.yml vs search)
   - If you couldn't verify something, say so

4. **Prioritize reliability over quantity**
   - 5 verified links are better than 20 guessed links
   - Users trust this tool to give accurate information
