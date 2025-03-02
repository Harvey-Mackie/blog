# How to write commits

## Introduction
Useful commit messages are key to managing a code-base, large or small. They allow you to track changes which will help when you need to create release notes, resolve bugs or view features. When commit messages are unclear, it can hinder the finding of certain changes and slow down the Pull Request stage.

When commit messages are unclear, it can hinder the finding of certain changes and slow down the Pull Request stage. 

For a reviewer, commit messages should document the flow of implementation which will allow for easier reading.

## How to Write Commits

### Key Rules
1. Keep the message concise - no more than 50 characters
2. Be specific - commit message should document WHAT changed, body should document WHY, code already documents HOW.
3. Use present tense - "Add" over "Added"
4. Structure commits to tell a story - will allow a reviewer to read each commit sequentially to understand the change and thought process.

### Commit Message Format (Conventional Commits) 
Use Conventional Commits to standardise how you structure commit messages. 

Conventional Commits is a specification which labels commits with a tag to identify if it is a feature, fix, documentation update, refactor or version upgrade etc.

Here are the key examples:
- feat: A new feature
- fix: A bug fix
- docs: Documentation changes only
- style: Changes that don't affect code meaning (formatting, missing semi-colons, etc.)
- refactor: Code changes that neither fix bugs nor add features
- test: Adding or correcting tests
- chore: Changes to build process or auxiliary tools
- perf: Performance improvements
- ci: Changes to CI configuration files and scripts

There is great documentation already at [Conventional Commits Documentation](https://www.conventionalcommits.org/en/v1.0.0/#specification)

### What does a Good Commit Look like?
 For example, a feature to implement secure logging might look like this:
```
Commit 1 - feat(security): implement sensitive data masking in logs
Commit 2 - feat(security): add pattern recognition for credit card numbers
Commit 3 - test(security): add integration tests for log masking
Commit 4 - docs(security): update README with log masking configuration
```
This clear progression shows how the feature was built in logical steps.


## How to manage commits

### In feature branch (Locally)
It's common for changes to be added after the initial commit. To reduce the noise, you should squash your commit.

For example, let's say you are implementing log masking.

Your first commit is `feat(security): implement sensitive data masking in logs`

Then you realise that you missed adding the masking for a certain format of logs e.g. JSON.

Your second commit would be `feat(security): implement sensitive data masking in logs for JSON format`

To improve this, you should squash these commits by running `git rebase -i HEAD~2`
```
0a1b2c3 feat(security): implement sensitive data masking in logs
4d5e6f7 feat(security): implement sensitive data masking in logs for JSON format
```

Into a single clean commit:

```
0a1b3c4 feat(security): implement sensitive data masking in logs
```
A more detailed example of interactive rebasing:
```
# Interactive rebase of the last 3 commits
git rebase -i HEAD~2

# In the editor that opens, you'll see something like:
# pick 0a1b2c3 feat(security): implement sensitive data masking in logs
# pick 4d5e6f7 feat(security): implement sensitive data masking in logs for JSON format
#
# Change to:
# pick 0a1b2c3 feat(security): implement sensitive data masking in logs
# squash 4d5e6f7 feat(security): implement sensitive data masking in logs for JSON format
#
# Save and close, then write a new commit message for the combined commit
```

### In master
The master branch should be lean and easy to triage. Rather than having multiple commits, you should squash the commits from the feature branch into one commit:

This will results in a single commit that represents the entire feature/fix

Rational behind this is to keep the main branch history clean and meaningful

As a result, each logical change appears as a single commit in the main branch


## Best Practice Workflow
An ideal workflow might look like this:
```
# Start a new feature
git checkout -b feature/log-masking

# Make regular commits as you work
git commit -m "feat(security): implement sensitive data masking in logs"
git commit -m "feat(security): implement sensitive data masking in logs for JSON format"
git commit -m "test(security): add tests for masking"

# Stay up to date with main branch
git fetch origin
git rebase origin/main

# Before creating PR, clean up commit history
git rebase -i HEAD~3

# Now you have clean commits
# Create PR in GitHub.
# When merging, select "Squash and merge" option
```

## Conclusion
Taking the time to write clear, structured commit messages and manage your commit history properly pays dividends throughout a project's lifecycle. Good commit practices:

Make debugging easier
Streamline code reviews
Facilitate automated versioning and release notes
Improve team communication
Create a valuable historical record of project decisions

By adopting conventional commits and proper history management techniques, you'll make your codebase more maintainable and your team more efficient.

Try this out, I'm sure your team will thank you and in a matter of time, they will adopt the same way of working.
