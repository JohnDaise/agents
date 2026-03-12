---
name: documentation-research
description: Systematic technical documentation research and analysis methodology. USE FOR researching technical documentation; extracting information from docs; synthesizing documentation into actionable insights; answering questions with proper citations. DO NOT USE FOR code analysis or generating new documentation.
---

# Documentation Research Methodology

Follow this systematic approach when researching and analyzing documentation:

## Research Process

### Step 1: Understand the Query
- Clarify what information is being requested
- Identify key terms and concepts to search for
- Determine scope (specific answer vs. comprehensive overview)
- Note any version-specific requirements

### Step 2: Locate Information
Search documentation using multiple strategies:
- **Keyword searches**: Use `grep_search` for exact terms
- **Semantic searches**: Use `semantic_search` for concepts and relationships
- **Table of contents**: Navigate documentation structure systematically
- **Related sections**: Follow cross-references and links
- **Online docs**: Use `fetch_webpage` for web-based documentation
- **Local docs**: Use `read_file` for local markdown/text files

### Step 3: Analyze & Verify
- Read relevant sections thoroughly
- Verify information accuracy by cross-referencing
- Check for version-specific details and compatibility notes
- Identify any conflicts, deprecations, or updates
- Note important caveats and warnings

### Step 4: Synthesize Response
- Organize information logically
- Provide clear, direct answers first
- Include supporting details and context
- Add practical examples where available
- Cite all sources with specific references

## Documentation Analysis Checklist

When analyzing documentation:

- [ ] Identify key concepts and terminology
- [ ] Map documentation structure and organization
- [ ] Note navigation paths and section relationships
- [ ] Extract code examples and usage patterns
- [ ] Identify configuration options and parameters
- [ ] Find best practices and recommendations
- [ ] Note version-specific information
- [ ] Highlight deprecations and migrations
- [ ] Collect warnings and caveats
- [ ] Record API details and signatures

## Output Format

### For Direct Questions

```
## Answer

[Clear, concise answer to the question]

## Details

[Comprehensive explanation with context]

## Examples

[Relevant code examples or usage patterns from docs]

## Source References

- [Documentation section/page title](URL or file path)
- [Additional reference](URL or file path)

## Important Notes

- [Any caveats, warnings, or version-specific information]
```

### For Comprehensive Research

```
## Overview

[High-level summary of the topic]

## Key Concepts

- **Concept 1**: Definition and explanation
- **Concept 2**: Definition and explanation

## Usage Patterns

[Common patterns and approaches with examples]

## Configuration & Options

[Relevant settings, parameters, API details]

## Best Practices

[Recommended approaches from documentation]

## Common Issues & Solutions

[Known gotchas and how to handle them]

## Related Topics

- [Link to related concept 1]
- [Link to related concept 2]

## Sources

- [Primary source](URL/path)
- [Additional sources](URL/path)
```

## Citation Best Practices

- Always provide specific section titles or headings
- Include URLs for online documentation
- Include file paths and line numbers for local docs
- Link to most authoritative/official source
- Note the documentation version when relevant
- Provide direct links to examples when available

## Information Quality Checks

Before presenting findings:
- Is the information from an official/authoritative source?
- Is it current (not outdated or deprecated)?
- Does it apply to the question's context/version?
- Are there any important caveats or limitations?
- Have you cross-referenced with other sections?
- Are examples complete and accurate?
