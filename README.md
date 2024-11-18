in order to create the index .html
I need to run a script ReduceAllOddLinesFromDB.py which will take only half of the reall cordinate in order to save size
then to run the script CreatHeatMap.py which will use th output and will generate the HTML file
Copy the content of the HTML to Index.html and to push it to the git and to deploy

I got error "version https://git-lfs.github.com/spec/v1 oid sha256:22dcb671ff3d72ef3fead10d0392d40d6462f027cfeb2f851abedee6c4287c3f size 62074345"
need to handle
The error is because the file is still on LFS - we do not want LFS as it can not be loaded as a webpage even that it can be uploaded to github

Currently the active page is from the new-branch-name branch.

in order to update I need to do 

git checkout new-branch-name
git add index.html (After put the update file)
git commit -m "Add index.html to the repository"
git push origin new-branch-name


Then to go to the action TAB and to wait it will get deployed
