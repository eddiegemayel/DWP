##DWP
##Eddie Gemayel
##Portfolio Deployment Plan
####Spin Up Production/ Staging Servers
1. Create new droplet(s) on Digital Ocean.
2. Once spun up , SSH into server(s) via terminal.
  * ssh root@Ipaddress
3. Change Password and create your admin user in the sudo group.
  * adduser gemayel
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
  * sudo pico apache.conf, then scroll to the bottom.  older -->(/etc/apache2/conf.d/security) (etc/apache2/sites-available/000-default.conf)
  * ServerName staging
3. Restart Server.
  * sudo service apache2 restart
4. Restrict Access
  * sudo pico /etc/apache2/conf-available/security 
  * Uncomment " < Directory /> "
  * Add
  * Options FollowSymLinks
  * Delete Deny from all
  * sudo service apache2 restart
5. Setting up Apache for Handling Multiple Sites
  * sudo pico /etc/apache2/sites-available/default
  * Change both occurrences of /var/www to /var/www/YourSite.com
  * sudo chown UserName /var/www
  * mkdir /var/www/YourSite.com


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



####Initializing hooks
1. Make sure admin has permissions to alter directory, and remove default index file if you havn't already.
  * sudo chown gemayel var/www/html
  * rm index.html
2. create/enter a repo directory in 'html/'' with admin permissions.
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
    * GIT_WORK_TREE=/var/www/html git checkout -f
  * chmod +x post-receive



####To push to release
6. Open new terminal window and navigate to projects folder to initialize
  * cd projects/
  * cd lab4/
  * git init
7. You can create empty read me file for first commit
  * touch readme.me
  * git add -A
  * git commit -m 'testing commit'
8. Add Remotes
  * git remote add productionServer ssh://gemayel@104.131.81.98/var/www/html/repos/portfolioProduction.git
  * git push productionServer master

####Install Postfix
1. SSH into server on terminal and enter these commands
  * sudo apt-get update
  * sudo apt-get install postfix
2. Then choose 'Internet Site'
3. Enter your domain name
4. Go into main.cf file to edit inet_interfaces
  * sudo pico /etc/postfix/main.cf
  * inet_interfaces = loopback-only or localhost
5. Restart then done.
  * sudo service postfix restart
  * sudo service apache2 restart