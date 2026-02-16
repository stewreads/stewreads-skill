---
name: stewreads
description: Optimize ebook creation workflow using Stewreads MCP tools. Use when user requests ebook generation, Kindle conversion, or wants to save content for reading on e-readers. Handles content formatting for Kindle devices, email management, and conversion orchestration.
---

# Stewreads Ebook Creation

## Overview

This skill optimizes the workflow for creating Kindle-optimized ebooks using the Stewreads MCP server. It ensures proper content formatting, email management, and seamless conversion to EPUB format for delivery to Kindle devices.

## Workflow

### Step 1: Check for Saved Kindle Email

Always start by checking if the user has a saved Kindle email address:

```
stew:get_saved_kindle_email()
```

- If email exists: Proceed directly to content generation
- If no email exists: Ask the user for their Kindle email address once

### Step 2: Generate Ebook Content

Before generating content, read the detailed formatting guidelines:

```
references/ebook-guidelines.md
```

Key points when generating:
- Follow the required structure: Title Page → Introduction → Chapters → Conclusion
- Apply strict formatting rules (no images, tables, or complex layouts)
- Keep paragraphs to 6-8 sentences maximum
- Use only allowed Markdown elements

**Display behavior:**
- If user wants to "send to Kindle" or "email to Kindle": Do NOT display content in Artifact. Generate and proceed directly to conversion.
- If user wants to preview or convert only: Display content in Artifact for review.

### Step 3: Convert and Email

Once content is generated, use:

```
stew:email_epub_to_kindle(
    markdown: "<generated_content>",
    title: "<descriptive_title>",
    email: "<kindle_email_or_null>",
    original_prompt: "<user_original_request>"
)
```

Parameters:
- `markdown`: The generated ebook content
- `title`: Clear, descriptive title for the ebook
- `email`: Use saved email if available, otherwise null (user will be prompted)
- `original_prompt`: The user's original request for context

## Common Scenarios

**Scenario 1: Quick conversion with saved email**
```
User: "Send an ebook about quantum mechanics to my Kindle"
→ Check saved email
→ Generate content following guidelines
→ Convert and email (no Artifact display)
```

**Scenario 2: New user without saved email**
```
User: "Create an ebook about investing"
→ Check saved email (returns null)
→ Ask for Kindle email address
→ Generate content
→ Convert and email with provided address
```

**Scenario 3: Preview before sending**
```
User: "Create an ebook about Python, I want to review it first"
→ Generate content following guidelines
→ Display in Artifact for review
→ Wait for user confirmation before conversion
```

## Available MCP Tools

- `stew:get_saved_kindle_email()` - Check for stored Kindle email
- `stew:email_epub_to_kindle()` - Convert markdown to EPUB and email to Kindle
- `stew:get_epub_system_prompt()` - Get system prompt (not needed with this skill)

## References

- `references/ebook-guidelines.md` - Detailed formatting and content generation rules
