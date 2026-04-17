---
name: coda-exporter
description: Export a Coda document to a standard Markdown file (.md). Use this skill whenever a user wants to download, convert, save, export, or back up a Coda page or document into Markdown. This skill automatically handles downloading the content and fixing internal anchor links to work correctly in the resulting Markdown file.
---

# Coda to Markdown Exporter

You are an expert at exporting Coda content to Markdown. 

When a user asks you to export a Coda document or page to Markdown, follow this exact procedure:

1. **Search for the document**:
   - If the user provides a Coda URL, use the Coda MCP `url_convert` tool (action: `decode`) to get the `docUri` and `uri`.
   - If the user provides a title or keywords, use the Coda MCP `search` tool to find the correct document or page and extract its `docUri` or `pageUri`.

2. **Read the content with Annotations**:
   - Use the `page_read` tool to fetch the raw Markdown content.
   - **Crucial**: You MUST set `markdownIncludeAnnotations: true` in the `page_read` parameters. This includes hidden `[[cl-elementId]]` tags before headers and blocks, which you will need for resolving links later.
   - Also set `contentTypesToInclude: ["markdown"]`.

3. **Identify internal links and resolve their anchors**:
   - Look for URLs starting with `https://coda.io/`.
   - A Coda URL often contains a fragment (e.g., `#_luQoBvzV`). The fragment minus the `_l` prefix is an element identifier. In this case, the identifier is `uQoBvzV`.
   - For each URL, use `url_convert` (action: `decode`) to verify its `docUri`. 
   - If the `docUri` matches the document you are exporting, it's an internal link.
   - To figure out *what heading* the link points to:
     - Search the fetched Markdown content for the corresponding annotation tag, which will look like `[[cl-xxxxIDENTIFIER]]` or exactly match the fragment identifier (e.g. `[[cl-TlqLQoBvzV]]` matches the fragment `#_luQoBvzV` because it contains `uQoBvzV`).
     - Look at the text immediately following that annotation tag. If it's a heading (e.g., `[[cl-TlqLQoBvzV]] ### Option 1`), the link should point to `Option 1`.
   - Rewrite the Coda URL into a standard Markdown anchor pointing to that heading text: `[Link Text](#option-1)`.
     - *Note: To generate the standard markdown anchor, convert the linked section's heading text to lowercase, remove punctuation, and replace spaces with hyphens.*

4. **Clean up the Markdown and Save**:
   - Remove all the `[[cl-elementId]]` annotations from the final Markdown text before saving. They are only for your reference to resolve links.
   - Save the cleaned-up Markdown content to a local `.md` file using the `Write` tool.
   - The filename should reflect the document's title (e.g., `Document_Title.md`).
   - Inform the user where the file was saved.

## Important Notes

- Do not modify links that point to *different* Coda documents (where `docUri` does not match the exported doc).
- Ensure the entire Markdown structure (headers, lists, code blocks) remains intact. Only modify the internal Coda anchor links.