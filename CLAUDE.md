# Global Claude Instructions

## Markdown & Documentation Files

When the user asks you to create documentation or markdown files, follow these rules:

1. **File location** — Create the file at `~/Documents/docs/<project-name>/<filename>.md` where `<project-name>` is the current project directory name and `<filename>` is a descriptive, kebab-case name for the feature or topic.

2. **Structure** — Include clear sections appropriate to the content. Default sections: Overview, Implementation Details, Design Decisions, Future Considerations, and any relevant code snippets or references.

3. **Tags** — Extract tags that describe the content. These can relate to technology stack, feature area, decision type, status, or any other relevant category. Use your judgement on what tags make sense based on the content.

4. **Frontmatter** — At the end of the file, include a YAML frontmatter block with the tags:
   ```yaml
   ---
   tags: [tag1, tag2, tag3]
   ---
   ```

5. **Tags index** — After creating the file, update `~/Documents/docs/_tags-index.md`:
   - For each new tag: add an entry with a brief description of what the tag means and a backlink to the file
   - For existing tags: just add the backlink to the existing entry
   - Keep the index organized and easy to scan
