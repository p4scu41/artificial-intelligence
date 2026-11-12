# Write documentation
I want you to write documentation that a developer can follow to implement this package in their own project. The reader is a developer who has never used the package; the documentation succeeds if they can go from zero to a working integration without reading the source code or asking anyone for help. Write for scanning: clear headings, short explanations, and code over prose wherever code communicates better. Explain why a step or option matters when it isn't self-evident, so developers can adapt the examples to their own project rather than copying them blindly. Create a Markdown file with all the content.

# Subagent review
Spawn a Codex agent using Solo MCP to review the entire PR. This should use the pr-review skill. When it finishes, get the result and address any findings using the same strategy used in the pr-review-fix-comments skill.

# Test plan
Create, as few as possible, test cases I can include in the testing plans to validate that the workflows the code changes impacted are working as expected.
