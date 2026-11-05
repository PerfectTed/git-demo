# Contributors

How we work in this repo without overwriting each other. Applies to everyone, and to any AI agent working on their behalf.

## 1. Get the repo (once)

```bash
cd ~/Documents
git clone https://github.com/PerfectTedTech/git-demo.git
cd git-demo                       # run every Git command from inside this folder
git status                        # should say: On branch main
```

Then add yourself to the [Contributors](#contributors) list at the bottom, as your first change.

## 2. Make a change

Every change, every time:

```bash
git switch main                              # move to the shared main branch
git pull                                     # get everyone's latest work
git switch -c type/short-description         # create your own branch (-c = create) and move onto it
# edit files and save
git status                                   # check your branch and what changed
git add <files>                              # choose what goes into the commit
git commit -m "type: short description"      # save a snapshot on your laptop
git push -u origin HEAD                      # send your branch to GitHub; prints a PR link
```

1. Open the link that `git push` printed. Check the base is `main`, add a reviewer, and click **Create pull request**.
2. If the reviewer asks for changes, edit, then `git add`, `git commit` and `git push` again on the same branch. The PR updates automatically.
3. Once approved, click **Merge pull request**, then **Confirm merge**, then **Delete branch**.
4. Tidy up:
   ```bash
   git switch main
   git pull
   git branch -d type/short-description   # delete your local copy of the branch
   ```

**If GitHub says "This branch has conflicts":** someone changed the same lines on `main` first. Click **Resolve conflicts** and edit the file so it's correct (usually keeping both changes). Delete the `<<<<<<<`, `=======` and `>>>>>>>` lines, then click **Mark as resolved** and **Commit merge**. If you're unsure which version is right, ask the person who changed `main`.

### With Claude or another AI agent

Start a change:

```
Follow CONTRIBUTORS.md. Switch to main and pull, then create and switch
to a branch called TYPE/SHORT-DESCRIPTION. [Describe the change.]
Commit, push, and open a PR into main. Give me the link. Don't merge.
```

When a PR has conflicts:

```
My PR has conflicts with main. Merge main into my branch, show me both
versions of anything that clashes, and wait for me to decide before
pushing.
```

## 3. Rules

- Never push or force-push to `main`. Every change goes through a branch and a PR.
- Run `git pull` on `main` before starting anything new.
- One purpose per PR, merged within a day or two.
- Branch names: `type/short-description`, e.g. `feat/keyvault-script`, `fix/sql-role-grant`, `docs/naming-conventions`.
- Commit messages: `feat:`, `fix:`, `docs:`, `chore:` or `refactor:`, then a short description in the imperative ("add", not "added").
- Never commit secrets, `.env` files or data exports. Secrets live in Key Vault.
- Someone other than the author reviews and merges.

## 4. For AI agents

Follow sections 2 and 3, plus:

- **Before every push**, check for conflicts with `main`:
  ```bash
  git fetch origin
  git merge-tree --write-tree --name-only origin/main HEAD
  ```
  - Exit code 0: no conflicts, so push.
  - Exit code 1: conflicts. **Stop.** Show the user the conflicting files, both versions, and who changed `main` (`git log origin/main -3 --format="%h %an %ar %s" -- <file>`). Wait for the user to decide, and never drop a teammate's changes.
- **Ask before:** pushing to `main`, force-pushing, anything that discards work (`reset --hard`, `clean`, `rebase`, `restore`, `stash drop`), or approving or merging a PR.
- Otherwise, get on with it. Flag only problems.

## 5. If something goes wrong

| Error | Fix |
|---|---|
| `Permission denied (publickey)` | `gh config set git_protocol https -h github.com`, then try again |
| `not a git repository` | `cd ~/Documents/git-demo` |
| `Updates were rejected` | `git pull`, then `git push` |
| `a branch named ... already exists` | `git switch <branch>` (without `-c`) |
| A full-screen editor opened | Press `Esc`, type `:wq`, press Enter (next time, use `-m`) |
| Anything else | Run `git status` and ask the team. Never use `--force`. |

## Contributors

<!-- One line each, at the bottom: - Firstname Lastname (@github-username), Role -->
- Diana Veletian (@PerfectTedDiana), Engine Room
- Fi