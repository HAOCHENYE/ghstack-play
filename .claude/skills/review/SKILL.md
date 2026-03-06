---
name: review
description: review-pr
disable-model-invocation: true
allowed-tools: Read,Write,Edit,MultiEdit,LS,Grep,Glob,Bash(cat:*),Bash(test:*),Bash(printf:*),Bash(jq:*),Bash(head:*),Bash(git:*),Bash(gh:*),mcp__github_inline_comment__create_inline_comment,
---

# PR Review

**REPO:** `${{ github.repository }}`
**PR NUMBER:** `${{ github.event.pull_request.number || github.event.issue.number }}`

## Accessing PR Content

Use the following commands to access the PR content:
- `gh pr view <PR NUMBER> --comments`
- `gh pr diff <PR NUMBER> --patch`

You can use multiple subagents to parallelize tasks. Each subagent should be told the PR title and description.

## PR Review Guide

You are in repository without that patch applied. **Do not apply it.**
Assume the patch will be built and tested by other GitHub Actions workflows.

### Focus Areas

- Code quality, style, and best practices
- Potential bugs, issues, incorrect logic
- Security implications
- `.claude/CLAUDE.md` compliance

### Review Process

1. For every issue, rate how significant it is. Make sure you are confident in your diagnosis.

2. Provide detailed feedback as PR comments, using inline comments for specific issues.

3. Use `gh pr comment ${{ github.event.pull_request.number }} --body "..."` for top-level summary. Keep it short.

4. Use `mcp__github_inline_comment__create_inline_comment` to highlight specific code issues.

5. Prefix all your GitHub comments with "Claude: ".

6. Use `gh pr view <PR NUMBER> --comments` output to make sure you don't comment about the same issue twice.

### Code Linking Format

When linking to code in inline comments, follow this format precisely (otherwise the Markdown preview won't render correctly):

```
https://github.com/${{ github.repository }}/blob/${{ github.event.pull_request.head.sha }}/README.md#L10-L15
```

