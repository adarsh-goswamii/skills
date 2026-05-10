# Global Claude Instructions

## Markdown & Documentation Files

When the user asks you to create documentation or markdown files, follow these rules.

First, create the file at the full path `~/Documents/docs/[project-name]/[filename].md` where `project-name` is the current project you're working on and `filename` is a descriptive name for the feature or topic.

Second, structure the markdown with clear sections like Overview, Implementation Details, Design Decisions, Future Considerations, and any relevant code snippets or references.

Third, extract tags that describe the content — these could relate to technology stack, feature area, decision type, status, or any other relevant category. You decide what tags make sense based on the content.

Fourth, at the end of the markdown file, include wiki-style links to tag files using double brackets syntax like `[[tag-name]]`.

Fifth, for each tag you use, check if a tag file exists at `~/Documents/docs/_tags/[tag-name].md`. If it doesn't exist, create it with a brief description of what that tag represents and include a section at the bottom called "Used in" with a backlink to the markdown file you just created, formatted as `[[project-name/filename]]`.

Sixth, if a tag file already exists, update the "Used in" section to add the backlink to the new markdown file.

Seventh, keep all tag files organized in the `~/Documents/docs/_tags/` folder. This way Obsidian will treat each tag as a node in your knowledge graph and show actual connections between feature markdowns and their related tags.
