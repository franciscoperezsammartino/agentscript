Rebuild the VS Code extension VSIX and replace the copy stored in the repo.

> **Note:** Run `/polish-changelog` before this command. The `prepackage` script runs
> `check-changelog.mjs`, which will fail if the new version has no CHANGELOG entry yet.

> **All shell commands must be run from the repository root.**

> **Note:** `packages/vscode/publish/` is gitignored and temporary — it will not appear in
> `git status` and does not need to be staged.

Use this command on a release-draft PR after the version in `packages/vscode/package.json` has been updated.

## Steps

1. Read `packages/vscode/package.json` and note the current `"version"` field.

2. Run the build pipeline in order (wait for each command to succeed before running the next):
   ```
   pnpm --filter @agentscript/vscode prepackage
   pnpm --filter @agentscript/vscode package
   ```
   If any command fails, stop immediately and report the error output. Do **NOT** proceed to file operations.

3. Find the newly built VSIX file inside `packages/vscode/publish/`. There should be exactly one `.vsix` file. If there are none or more than one, stop and report.

   Verify that the VSIX filename contains the version string noted in Step 1 (e.g. `agentscript-2.4.0.vsix`). If it does not, stop and report.

4. Ensure the destination directory exists and remove any stale VSIX files:
   ```
   mkdir -p packages/vscode/vsix-versions/
   find packages/vscode/vsix-versions/ -name "*.vsix" -delete
   ```

5. Copy the new VSIX from `packages/vscode/publish/` into `packages/vscode/vsix-versions/`.

6. Stage the directory — run:
   ```
   git add packages/vscode/vsix-versions/
   ```
   Do **NOT** commit.

7. Print the name and file size (in KB) of the VSIX that was staged.
