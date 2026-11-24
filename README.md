# Dantas Wiki

This is my personal Obsidian vault: a second brain for collecting, connecting, and developing knowledge about my life, interests, work, and projects. It is intended to grow over time, so its structure should remain useful without becoming rigid.

Open this directory as a vault in [Obsidian](https://obsidian.md/). Notes are ordinary Markdown files, and folders provide broad areas of organization. Existing areas include `Automation`, `contabilidade`, `Gastronomy`, and `Mulesoft`; add or adapt areas as the vault grows.

## Customers

- [[OneDigital]]
- [[True Manufacturing]]
- [[MLC]]
- [[Proponent]]
- [[INX]]

## Working in this vault

These instructions apply to people and agents creating or editing notes here:

- Search the vault before creating a note. Extend an existing note when it already covers the subject; create a separate note when the topic is useful on its own.
- Put notes in an appropriate existing folder when possible. Create a folder when it helps navigation, not for every topic. Use descriptive, stable filenames and avoid duplicate note names where practical.
- Keep notes focused and easy to scan, with clear headings and enough context to understand them later. Preserve the author's language and voice; use Portuguese, English, or both as appropriate to the material.
- Connect related notes where the relationship is useful. Explain the connection in the surrounding text instead of adding links solely to populate the graph. Do not invent facts or relationships.
- Preserve existing content, formatting, and conventions unless a change is needed for the task. Do not reorganize, rename, or delete unrelated notes without a reason.
- Treat personal information with care. Do not add secrets, credentials, or sensitive personal details unless explicitly requested, and do not assume a local vault or Git repository is private or encrypted.

## Linking notes

Use Obsidian wikilinks for links within the vault. Confirm that the target exists and that the link resolves to the intended note; use a vault-relative path when a filename is ambiguous. Omit `.md` from wikilinks to notes.

- Link to a note: `[[Healthy Diet]]`.
- Give a link a readable label: `[[Healthy Diet|weekly meal prep]]`.
- Link to a heading: `[[Healthy Diet#Shopping list (weekly batch)]]`. Check that the heading exists and matches the target.
- Disambiguate a note with its path: `[[Gastronomy/Beans|beans recipe]]`.
- Embed an existing note or attachment when useful: `![[Gastronomy/Beans]]` or `![[attachment.png]]`.

Use standard Markdown links for external websites, and include a source link when recording information from an outside source. Do not create placeholder links to notes that do not exist unless the user deliberately wants an unresolved link as a future reminder. Use tags sparingly for broad categories or workflows; tags complement links rather than replace meaningful connections.

When renaming or moving a note, prefer doing so in Obsidian with automatic internal link updates enabled. If working outside Obsidian, find and update affected links manually, including path-qualified links, embeds, and links to headings or blocks. When changing a heading or block identifier, update links that point to it. Check the affected links afterward and avoid breaking existing references.

## Properties and attachments

Use YAML frontmatter properties when they add practical value, such as aliases, dates, or status. Follow existing property names and types when present; do not impose a mandatory schema or add empty metadata to every note. Keep attachments in a relevant existing location or a clearly named folder, use descriptive filenames, and verify that their links or embeds resolve.

## Final check for agents

Before finishing, review the changed notes for accuracy, readable Markdown, and useful context. Verify new and modified internal links, embeds, and referenced headings or blocks against actual vault files. If a rename or move was involved, check inbound references too. Report any intentionally unresolved links or other limitations to the user.
