# GitHub Collaboration Workflow — SMRS

## 1. Team responsibilities
| Area | Backend developer | Frontend developer |
|---|---|---|
| FastAPI, database, migrations | Owner | Consumer of API contract |
| HTML, CSS, JavaScript pages | API support | Owner |
| API contract | Publishes and maintains | Follows and reports gaps |
| Integration testing | Joint | Joint |
| Pull request review | Reviews frontend API usage | Reviews agreed frontend structure |

## 2. Branch strategy
Recommended branches:
- `main`: stable, reviewed code.
- `develop`: integrated development branch.
- `feat/backend-...`: backend feature branches.
- `feat/frontend-...`: frontend feature branches.
- `fix/...`: bug fixes.

Do not commit directly to `main`.

## 3. Issue workflow
1. Create an Issue with a clear title, description, and acceptance criteria.
2. Assign one owner.
3. Add labels such as `backend`, `frontend`, `documentation`, `bug`, or `priority-high`.
4. Create a branch linked to the Issue.
5. Commit small, understandable changes.
6. Push the branch and open a Pull Request.
7. Review and test the changes.
8. Merge only after checks and review pass.

## 4. Branch and commit examples
```bash
git checkout develop
git pull origin develop
git checkout -b feat/frontend-login

git add .
git commit -m "feat(frontend): create login page"
git push -u origin feat/frontend-login
```

For a backend task:
```bash
git checkout develop
git pull origin develop
git checkout -b feat/backend-auth
```

## 5. Pull Request checklist
- [ ] PR is linked to an Issue.
- [ ] Scope is limited to the Issue.
- [ ] No `.env`, passwords, API keys, or private data included.
- [ ] Relevant tests/manual checks completed.
- [ ] API contract/documentation updated if it changed.
- [ ] Reviewer has checked the changes.
- [ ] No unrelated files or generated secrets committed.

## 6. Issue template
### Summary
What needs to be built?

### Scope
What is included and excluded?

### Acceptance criteria
- [ ] Expected behavior 1
- [ ] Expected behavior 2
- [ ] Error/permission cases handled

### Dependencies
Which API, page, database model, or other Issue is required?

### Testing
How will the team verify completion?

## 7. Coordination rule
The backend developer must notify the frontend developer before changing a published API contract. Update the API documentation and example payloads in the same change where practical.
