

## Basic git cmds

`git fetch`

`git checkout main`

`git pull`

`git tag 0.2.0`

`git push --tags`

while working on another branch and shifting to another branch (main -> 11-terraform-cloud-backend) use the below cmds:

`git save .  `  // temp save

`git stash save`

`git fetch origin`

`git checkout 11-terraform-cloud-backend`

`git stash apply`

### Edit tags to overwrite old tag with new one
 
*In Git, tags are typically considered immutable, meaning you can't directly edit a tag. However, you can create a new tag and delete the old one if you want to effectively replace it. Here's a step-by-step guide:*

1. Create a New Tag:

`git tag -a 'new_tag_name' -m "Your message"`


2. Push the New Tag to Remote:

`git push origin 'new_tag_name'`

3. Delete the Old Tag Locally:

`git tag -d 'old_tag_name'`


4. Delete the Old Tag on Remote:

`git push origin --delete 'old_tag_name'`

This will delete the old tag on the remote repository.


### Cloning a git repo

- `git clone https://github.com/ExamProCo/terratowns_mock_server.git`
- `ls -la` lists all the folders(ls) with permission access(-la) 
- `rm -rf .git` remove with force .git file