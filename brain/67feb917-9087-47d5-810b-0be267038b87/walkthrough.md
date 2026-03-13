# Context Provider Utility Walkthrough

I have implemented a utility to help you provide context to external AI assistants. This tool aggregates your project's structure, documentation, and configuration into a single JSON file.

## Changes Made

### backend/scripts/get_context.py
Created a Python script that:
- Generates a JSON file tree of the project.
- Embeds the content of `README.md`, `CLAUDE.md`, and all files in the `docs/` folder.
- Includes dependency information from `package.json` and `requirements.txt`.
- Includes `.env.example` to show configuration requirements without exposing secrets.

### README_CONTEXT.md
Added clear documentation on how to use the script and what it provides.

## Verification Results

I verified the script by running it and checking the output:

```bash
cd backend
python3 scripts/get_context.py > ../context.json
```

The resulting `context.json` was validated as a correct JSON file and contained all expected sections:
- `file_tree`: Properly nested representation of the project structure.
- `documentation`: Full text of all documentation files.
- `configuration`: Parsed `package.json` and raw text of `requirements.txt` and `.env.example`.

## How to use it

You can generate the context file anytime by running:
`cd backend && python3 scripts/get_context.py > ../context.json`

Then, you can simply upload `context.json` to any AI assistant to give it full project context.
