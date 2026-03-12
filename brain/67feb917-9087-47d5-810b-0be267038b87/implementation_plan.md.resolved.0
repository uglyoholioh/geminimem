# Context Provider Utility Implementation Plan

To help the user provide context to external AIs, I will create a utility that aggregates key project information into a structured JSON format. This information will include the file tree, documentation, and core configuration files.

## Proposed Changes

### [Backend]
I will create a script in the `backend/scripts` directory that handles the context aggregation.

#### [NEW] [get_context.py](file:///Users/oli/Desktop/CraftCanvas/backend/scripts/get_context.py)
This script will:
- Walk the project directory and generate a JSON file tree (skipping `node_modules`, `.venv`, `.git`, etc.).
- Read and include the content of `CLAUDE.md`, `README.md`, and all files in the `docs/` directory.
- Include dependencies from `backend/requirements.txt` and `frontend/package.json`.
- Include `.env.example` to show required environment variables.
- Output the result to `stdout` or a specified JSON file.

### [Documentation]
#### [NEW] [README_CONTEXT.md](file:///Users/oli/Desktop/CraftCanvas/README_CONTEXT.md)
Documentation on how to use the `get_context.py` script and what information it provides.

## Verification Plan

### Automated Tests
I will run the script and verify that the output is valid JSON and contains the expected sections.

```bash
cd backend
python3 scripts/get_context.py > context_test.json
# Verify JSON validity
python3 -m json.tool context_test.json > /dev/null
```

### Manual Verification
- Review the generated `context_test.json` to ensure no sensitive information (like actual `.env` contents) is included.
- Verify that the file tree correctly reflects the project structure.
- Check that the documentation content is correctly embedded.
