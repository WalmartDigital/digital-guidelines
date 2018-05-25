---
title: "Commit Notation"
description: "Recommendation about how to commit a git message"
---

## Commit anotation
If you use a software with "smart commit" feature (like JIRA) we recommend to declare the id of the card for information about the story associated with the commit.

The recommedation for how to structure a git message are:

### Story Card Id (like JIRA Id)
When the story card is present, is easy to get the complete information about the objetive of your commit, so we strongly recommend to add in the git message.

### Commit category
Is useful to categorize a message because the other teams which review the change, can get the "big idea" of the change and reduce the files to focus for the review

We have the 3 categories you can use:
- **feature**: A new feature added to the project
- **fix**: A fix over a feature or a modification to fix a bug or issue
- **upgrade**: An upgrade over a feature which already exist to add new functionalities


### Structure

**[ CARD_ID|no-history ]** (**[ category ]**): **[ inline description ]**

#### Example:
JIRA-1 (feature): add deployer api server && up test > 50%
