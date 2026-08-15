# ask_doctor — Copilot Skill endpoint (GitHub Pages)

What this is
- A static Copilot Skill descriptor (skill.json) and simple landing page you can publish via GitHub Pages.
- When published, the JSON is a publicly reachable skill endpoint suitable for registering or pointing Copilot to.

Recommended repo layout (place these files in the repository):
- docs/ask_doctor/skill.json
- docs/ask_doctor/index.html
- (optional) docs/ask_doctor/README.md

Publishing steps (Docs folder approach)
1. Create a GitHub repo (or use an existing one).
2. Add the files under `docs/ask_doctor/` in the repository root.
3. Commit and push to the default branch (e.g., `main`).
4. In the repository settings -> Pages, set "Source" to the default branch and folder `docs/`.
5. Save. After a few minutes your files will be available at:
   `https://<your-github-username>.github.io/<your-repo>/ask_doctor/skill.json`

Alternative: gh-pages branch
- Put the files at the repository root on a `gh-pages` branch, enable Pages from that branch. The path will be:
  `https://<your-github-username>.github.io/<your-repo>/ask_doctor/skill.json`

Verify endpoint
- curl https://<your-github-username>.github.io/<your-repo>/ask_doctor/skill.json

Notes & next steps
- I preserved your global guardrails and the required disclaimer. The skill JSON is intentionally static and descriptive — it does not contain a conversational engine or private APIs.
- If you want, I can:
  - Add an OpenAPI-compatible wrapper or a minimal serverless function to host dynamically.
  - Produce a registration manifest or instructions for registering this skill with a specific Copilot/Skills registry (if you provide the registry details).
