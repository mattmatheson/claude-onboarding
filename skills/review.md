Review the current git diff (staged + unstaged changes) for:

1. **Bugs or logic errors** — race conditions, off-by-one, null handling
2. **Security issues** — hardcoded secrets, injection vectors, XSS, exposed API keys
3. **Performance concerns** — unnecessary re-renders, N+1 queries, missing indexes
4. **Code style** — inconsistencies with existing codebase patterns
5. **Missing error handling** — at system boundaries (user input, API calls, file I/O)

Be direct. Reference specific files and line numbers. If it looks good, say so briefly. If there are issues, list them ranked by severity.
