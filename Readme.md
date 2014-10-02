##DWP
##Eddie Gemayel
##Portfolio Deployment Plan
####Spin Up Server
1. Create new droplet on Digital Ocean
2. Once powered up , SSH into server via terminal
  * ssh root@104.131.81.98.
3. Change Password and create your admin user in the sudo group
  * adduser gemayel sudo 
4. Re-login as newly created admin user
5. Run commands to update your server
  * sudo apt-get update
  * sudo apt-get upgrade
  * sudo apt-get update

####Setup Apache
1. Run command
  * sudo apt-get install apache2

####Setup Github
1. Install git core
  * sudo apt-get install git-core
2. Setup the Github congifs like so
  * git config --global user.name gemayel
  * git config --global user.email eddiegem16@gmail.com
3. Get github public key
  * ssh-keygen -t rsa -C "eddiegem16@gmail.com"
4. Copy github key from id_res.pub file
5. Paste it into SSH keys under settings in github account
6. SSH into github to check it works. It will then kick you out.
  * ssh git@github.com
  * Hi eddiegemayel! You've successfully authenticated, but GitHub does not provide shell access. Connection to github.com closed.

####Push github repo up to the live site
1. First, change ownership of 'www' directory so we can put git repo into it.
  * sudo chown gemayel -R ../www/
2. Delete old index.html file
  * rm ./index.html
3. Next, initialize Git.
  * git init
4. Create remote to get from Github repo
  * git remote add github git@github.com:git@github.com:eddiegemayel/DWP.git
5. Pull from that repo
  * git pull github master