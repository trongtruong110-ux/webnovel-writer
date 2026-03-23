# Instructions for AI Agents

You are authorized to work in this project following these specific constraints:

## 1. Trust and Tool Use
- **Bash Authorization**: You are authorized to run all Python scripts located in `webnovel-writer/scripts/` to manage the project state, index data, and extract context.
- **Workflow Integrity**: Do not skip steps in the writing workflow defined in `webnovel-writer/skills/webnovel-write/SKILL.md`.
- **Minimal Prompts**: If the user has trusted this directory, proceed with tool calls for project-internal operations (listing files, reading state, running index commands) without excessive hesitation.

## 2. Language Rules (MANDATORY)
- **Response Language**: Vietnamese (Tiếng Việt) for all communication.
- **Novel Content**: English for all story text.
- **Image Prompts**: English for all Easy Diffusion prompts.

## 3. Data Integrity
- Always ensure `index.db` and `state.json` are updated after writing a chapter.
- Always generate Visual Prompts for every scene identified by the Data Agent.
