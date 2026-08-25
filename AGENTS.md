# Holly Recruiting — Agent Instructions

This repository powers Holly Recruiting's public GitHub organization profile. Keep changes small, clear, and focused on the content under `profile/`.

## Boundaries

### Always

<always>

- Make the smallest change that accomplishes the task.
- Review the rendered profile and assets when changing public-facing content.
- Treat accessibility as a hard release requirement for every public-facing change. Before
  committing, audit the rendered README and assets for a logical heading structure, unique and
  descriptive control/link names, accurate image alternatives, correctly associated SVG titles and
  descriptions, sufficient contrast, keyboard-operable interactions, readable text at GitHub width,
  and unnecessary screen-reader duplication. Fix every discovered accessibility regression; passing
  visual or XML validation alone is not sufficient.
- Preserve the existing Markdown and SVG style, avoid unrelated reformatting.
- Validate changed files before committing and pushing.
- Commit and push directly to `main` unless explicitly asked to use a branch.

</always>

### Ask first

<ask>

- Adding workflows, automation, or third-party integrations.
- Deleting files or removing public-facing content.
- Creating new top-level directories.

</ask>

### Never

<never>

- Commit secrets, credentials, private company information, or candidate data.
- Use `--no-verify`, `--no-gpg-sign`, or disable a check to force a commit through.
- Add production application code or internal engineering documentation to this public profile repository.

</never>
