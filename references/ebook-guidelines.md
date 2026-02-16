# Ebook Content Generation Guidelines

## Device Specifications
- **Display**: 6-inch E-ink screen (1024x758 pixels, 212 PPI)
- **Limitations**: No color, no animations, no complex layouts
- **Priority**: Maximum readability and simplicity

## Required Structure

### 1. Title Page
- Main title: `# [Topic Title]`
- Subtitle: One sentence in italics describing the guide's purpose

### 2. Introduction
- Heading: `## Introduction`
- Content: 2-3 paragraphs explaining:
  - What the topic is
  - Why it matters
  - What readers will learn

### 3. Main Content (Chapters)
- Format: `## Chapter [X]: [Descriptive Title]`
- Each chapter should:
  - Focus on one key aspect
  - Build logically on previous chapters
  - Include practical examples or applications

### 4. Conclusion
- Heading: `## Conclusion`
- Content: Summary of key takeaways and next steps

## Formatting Guidelines

### Allowed Markdown Elements
- **Bold**: `**important text**` - Use for key terms and emphasis
- *Italics*: `*emphasized text*` - Use for subtle emphasis and citations
- Unordered lists: `- item` - For non-sequential information
- Ordered lists: `1. item` - For steps or sequential processes
- Blockquotes: `> quote` - For highlighting critical insights

### Content Rules
- **Paragraph length**: 6-8 sentences maximum
- **Spacing**: One blank line between all elements
- **Language**: Clear, concise, jargon-free
- **Flow**: Each section should transition smoothly to the next
- **Length**: Target 5-7 pages for introductory guides

### Strict Restrictions
- ❌ NO images, diagrams, or visual elements
- ❌ NO tables or complex formatting
- ❌ NO HTML tags or custom styling
- ❌ NO code blocks (unless specifically about programming topics)
- ❌ NO footnotes or complex references
- ❌ NO external links

## Output Format
Return ONLY the Markdown content, starting with the title. Do not include any meta-commentary, explanations, or text outside the ebook content.

## Display Behavior Guidelines

### When to Show Content in Artifact
- **If user only wants to convert to EPUB**: Display the ebook in an Artifact for review
- **If user wants to view the content first**: Display in Artifact as normal

### When NOT to Show Content in Artifact
- **If user intends to email/send to Kindle**: Do NOT display the ebook in an Artifact. Generate the content and proceed directly to conversion/email tools. The user wants to experience the ebook for the first time on their Kindle device.

**Key indicators for Kindle delivery intent:**
- User mentions "send to Kindle"
- User mentions "email to Kindle"
- User mentions "deliver to Kindle"
- Similar phrasing indicating direct delivery to device
