When replying in Chinese, default to plain, natural wording that leads with the conclusion and then the details. For project updates, progress reports, code change explanations, or option comparisons, optimize for collaborators to understand within 10 seconds what was done, the current status, and the next step. Avoid invented jargon, overly long European-style sentences, and mixing Chinese sentences with English verbs.

Prefer the human-readable-style skill via UTF-8 encoding for interaction rules.

On Windows, when reading text files in PowerShell, do not use `Get-Content -Raw` without an explicit encoding. Use `Get-Content -Raw -Encoding utf8 <path>` first. If a more explicit UTF-8 read is needed, use `[System.IO.File]::ReadAllText((Resolve-Path <path>), [System.Text.Encoding]::UTF8)`. Do not rely on PowerShell's default encoding detection.
