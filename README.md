# winicpdemos

Git global setup

git config --global user.name "Winton"
git config --global user.email "wintonjkt@gmail.com"

Create a new repository

git clone http://prd-master1/Winton/sprdemo.git
cd sprdemo
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master

Existing folder

cd existing_folder
git init
git remote add origin http://prd-master1/Winton/sprdemo.git
git add .
git commit -m "Initial commit"
git push -u origin master

Existing Git repository

cd existing_repo
git remote add origin http://prd-master1/Winton/sprdemo.git
git push -u origin --all
git push -u origin --tags
