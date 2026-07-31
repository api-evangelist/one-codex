---
name: Organize samples into a project and share it
description: >-
  Create a One Codex project, tag and attach samples, and share the project with
  collaborators.
api: openapi/one-codex-openapi-original.json
operations:
  - post_projects_instances
  - post_tags_instances
  - patch_samples_self
  - post_projects_add_user
  - get_projects_members
---

# Organize and share a project

Base URL: `https://app.onecodex.com/api/v1/`. Auth: `X-API-Key: $ONE_CODEX_API_KEY`.

## Steps

1. **Create a project.** Call `post_projects_instances` with a name to create a
   project container.
2. **Create tags.** Call `post_tags_instances` to define reusable labels.
3. **Attach samples.** Update a sample with `patch_samples_self` to set its `project`
   and add `tags`, organizing your uploads.
4. **Share.** Call `post_projects_add_user` to grant a collaborator access; confirm
   membership with `get_projects_members`. Use `post_projects_change_sharing` to adjust
   the project's sharing level.

## Rules

- Errors follow `{"message": ..., "status": <code>}`; `403` means you lack rights on
  the project or sample.
- Removal is symmetric: `post_projects_remove_user` revokes a member.
- Respect the 10 req/s limit and paginate member/sample listings with `page`/`per_page`.
