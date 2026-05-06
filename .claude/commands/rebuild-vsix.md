Rebuild the VS Code extension VSIX and replace the copy stored in the repo.

Use this command on a release-draft PR after the version in `packages/vscode/package.json` has been updated.

## Steps

1. Read `packages/vscode/package.json` and note the current `"version"` field. You will use this to verify the output file.

2. Run the full build pipeline in order (wait for each command to succeed before running the next):
   ```
   pnpm build --filter @agentscript/vscode...
   pnpm --filter @agentscript/vscode prepackage
   pnpm --filter @agentscript/vscode package
   ```
   If any command fails, stop immediately and report the error output. Do **NOT** proceed to file operations.

3. Find the newly built VSIX file inside `packages/vscode/publish/`. There should be exactly one `.vsix` file. If there are none or more than one, stop and report.

4. Remove **all** existing `.vsix` files from `packages/vscode/vsix-versions/` (the directory may be empty — that is fine).

5. Copy the new VSIX from `packages/vscode/publish/` into `packages/vscode/vsix-versions/`.

6. Stage the directory — run:
   ```
   git add packages/vscode/vsix-versions/
   ```
   Do **NOT** commit.

7. Print the name and file size (in KB) of the VSIX that was staged.
