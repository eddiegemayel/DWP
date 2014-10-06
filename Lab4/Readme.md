##DWP
##Eddie Gemayel
##Portfolio Deployment Plan
####Spin Up Production/ Staging Servers
1. Create new droplet(s) on Digital Ocean.
2. Once spun up , SSH into server(s) via terminal.
  * ssh root@Ipaddress
3. Change Password and create your admin user in the sudo group.
  * adduser gemayel sudo 
4. Re-login as newly created admin user.
5. Run commands to update your server(s).
  * sudo apt-get update
  * sudo apt-get upgrade
  * sudo apt-get update
6. Update System level Packages.
  * sudo aptitude update
  * sudo aptitude safe-upgrade
  * sudo reboot
  


####Setup Apache
1. Run command.
  * sudo apt-get install apache2
2. Configure Server name so apache2 can restart successfully.
  * sudo pico /etc/apache2/conf.d/security
  * ServerName staging
3. Restart Server.
  * sudo service apache2 restart


####Setup Github
1. Install git core.
  * sudo apt-get install git-core
2. Setup the Github congifs.
  * git config --global user.name gemayel
  * git config --global user.email eddiegem16@gmail.com
3. Get github public key
  * ssh-keygen -t rsa -C "eddiegem16@gmail.com"
4. Copy github key from id_res.pub file.
  * less /home/gemayel/.ssh/id_rsa.pub
5. Paste it into SSH keys under settings in github account.
6. SSH into github to check it works. It will then kick you out.
  * ssh git@github.com
  * Hi eddiegemayel! You've successfully authenticated, but GitHub does not provide shell access. Connection to github.com closed.




####Initialize git
1. First, change ownership of 'www' directory so we can put git repo into it.
  * sudo chown gemayel -R ../www/
2. Delete old index.html file.
  * rm ./index.html
3. Next, initialize Git while in the www folder.
  * git init

####Initializing hooks
1. Make sure admin has permissions to alter directory, and remove default index file if you havn't already.
  * sudo chown gemayel ./www/
  * rm index.html
2. Move to /var directory and create/enter a repo directory with admin permissions.
  * cd /var
  * sudo mkdir repos
  * sudo chown gemayel repos
  * cd repos/
3. Make a '.git' folder in your repos directory, then head inside the directory.
  * mkdir portfolioProduction.git
  * cd portfolioProduction.git/
4. Formally initialize bare git, and go into hooks directory
  * git init --bare
  * cd hooks/
5. Inside hooks, create executable file and change it's permissions so it can run succesfully
  * pico post-receive
  * Then type the following inside that file: 
    * #!/bin/sh
    * GIT_WORK_TREE=/var/www git checkout -f
  * chmod +x post-receive