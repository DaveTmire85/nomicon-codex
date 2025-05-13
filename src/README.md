
# Rustonomicon Canonical Dataset (Structured JSONL + SQLite)

This repository contains a structured, query-ready dataset derived from [The Rustonomicon](https://github.com/rust-lang/nomicon), the definitive guide to unsafe Rust. It provides a programmatically ingestible version of the book in both JSONL and SQLite formats.

## What This Is

This dataset transforms the Rustonomicon’s original Markdown source into a line-delimited JSONL file and a relational SQLite database, with canonical identifiers, trust levels, tags, and full content extracted per chapter or section. It is designed for use in AI agents, intelligent tutoring systems, static analyzers, and automated documentation workflows.

**Included Files:**

- `nomicon_canon_nodes.jsonl` – JSON Lines file of each section with structured metadata
- `nomicon.sqlite3` – SQLite database with the same entries, indexed for fast querying
- `LICENSE-*` – License declarations from the original Rustonomicon repository (MIT or Apache 2.0)

## How It Was Derived

1. Original content was pulled from the [Rustonomicon GitHub repository](https://github.com/rust-lang/nomicon) in Markdown format.
2. Each `.md` file was parsed to extract:
   - Title (from the first header)
   - Section name (from filename)
   - Raw Markdown content
   - Detected tags based on keywords like `unsafe`, `drop`, `lifetime`, etc.
   - A `trust_level` (`safe` or `hazardous`) inferred by presence of critical keywords
   - A `checksum` (SHA-256) for content verification and canonical trust enforcement
3. Data was saved to JSONL, and also inserted into a normalized SQLite database.

## How to Use It

### JSONL Format

Each line is a JSON object with the following structure:

```json
{
  "id": "canon::rust::nomicon::aliasing",
  "title": "Aliasing",
  "section": "aliasing",
  "tags": ["unsafe", "aliasing"],
  "trust_level": "hazardous",
  "content": "Raw pointers are not subject to the borrow checker...",
  "checksum": "sha256-<digest>"
}
```

You can:
- Stream it into LLM pipelines for domain-specific memory
- Use tags to scope behavior (e.g. trigger strict mode if `unsafe`)
- Perform textual analysis or search

### SQLite Format

The database contains a single table: `canon_nodes`.

```sql
SELECT * FROM canon_nodes WHERE tags LIKE '%unsafe%' LIMIT 10;
```

Fields:
- `id` (TEXT, PRIMARY KEY)
- `title` (TEXT)
- `section` (TEXT)
- `tags` (TEXT, CSV string)
- `trust_level` (TEXT)
- `content` (TEXT)
- `checksum` (TEXT)

Use it in:
- Embedded CLI tools
- LSP extensions
- Safety verification frameworks

## General Applications (Beyond MMF/Sigil)

- **AI Tutors:** Preload canonical Rust unsafe lore into an LLM agent for smart tutoring.
- **Code Review Automation:** Use tags and trust levels to build an unsafe usage analyzer.
- **Docs-as-Data:** Convert Rust documentation into structured, indexable systems for search and analysis.
- **Compiler/IDE Plugins:** Surface context-aware links to Nomicon lore inline as devs write unsafe code.

## License

This dataset is derived from [The Rustonomicon](https://github.com/rust-lang/nomicon), © The Rust Project Developers, and is licensed under either:

- MIT License
- Apache License, Version 2.0

You may redistribute, modify, or build upon this work under the terms of those licenses. This dataset makes no content changes to the source material—only structural and format transformations.

---

For questions or integration help, reach out to the project maintainer or open an issue.
