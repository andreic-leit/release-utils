---
name: create-release
description: >
  Creates a GitHub release from the files introduced or modified in a specific
  commit. Use this skill when asked to "create a release", "release a commit",
  or "publish files from a commit". The skill tags the commit, creates a GitHub
  release, and uploads the relevant files as release assets.
---

## Overview

This skill automates the end-to-end process of publishing a GitHub release that
packages the files touched by a given commit.

## Inputs

| Input | Description |
|-------|-------------|
| `commit_sha` | The full or short SHA of the source commit. Defaults to `HEAD`. |
| `tag_name` | The tag to create (e.g. `v1.2.3`). Required. |
| `release_name` | Human-readable title for the release. Defaults to `tag_name`. |
| `release_notes` | Body text / changelog for the release. Optional. |
| `draft` | Set to `true` to create a draft release. Defaults to `false`. |
| `prerelease` | Set to `true` to mark the release as a pre-release. Defaults to `false`. |

## Procedure

### 1. Resolve the commit

```bash
# If no SHA was supplied, use HEAD
COMMIT_SHA=$(git rev-parse "${commit_sha:-HEAD}")
echo "Target commit: $COMMIT_SHA"
```

### 2. Collect files changed in the commit

```bash
# List files added or modified by the commit
git diff-tree --no-commit-id -r --name-only "$COMMIT_SHA" > /tmp/release_files.txt
echo "Files to include in release:"
cat /tmp/release_files.txt
```

### 3. Create the tag at the commit

Use the GitHub API (or the `gh` CLI) to create an annotated tag pointing to the
resolved commit.

```bash
gh api repos/{owner}/{repo}/git/tags \
  --method POST \
  --field tag="$TAG_NAME" \
  --field message="Release $TAG_NAME" \
  --field object="$COMMIT_SHA" \
  --field type="commit"

# Create the corresponding ref
gh api repos/{owner}/{repo}/git/refs \
  --method POST \
  --field ref="refs/tags/$TAG_NAME" \
  --field sha="$COMMIT_SHA"
```

### 4. Create the GitHub release

```bash
gh release create "$TAG_NAME" \
  --title "${release_name:-$TAG_NAME}" \
  --notes "${release_notes:-}" \
  ${draft:+--draft} \
  ${prerelease:+--prerelease} \
  --target "$COMMIT_SHA"
```

### 5. Upload release assets

For each file listed in `/tmp/release_files.txt`, check out the file at the
target commit and upload it as a release asset.

```bash
while IFS= read -r file; do
  # Restore the file at the exact commit state
  git show "$COMMIT_SHA:$file" > "/tmp/$(basename "$file")"
  gh release upload "$TAG_NAME" "/tmp/$(basename "$file")" --clobber
done < /tmp/release_files.txt
```

### 6. Verify

```bash
gh release view "$TAG_NAME"
```

## Notes

- The `gh` CLI must be authenticated (`gh auth login` or the `GITHUB_TOKEN`
  environment variable must be set).
- Binary files are handled correctly by `git show`; no special treatment is
  needed.
- If multiple files share the same basename, use the full relative path as the
  asset name to avoid collisions.
- To include **all** files in the repository at the commit (not just the diff),
  replace step 2 with:
  ```bash
  git ls-tree -r --name-only "$COMMIT_SHA" > /tmp/release_files.txt
  ```
