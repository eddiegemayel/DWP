##DWP
##Eddie Gemayel
##Portfolio Deployment Plan
####Spin Up Server
1. Create new droplet on Digital Ocean
2. Once powered up , SSH into server via terminal
..* ssh root@104.131.81.98.
3. Change Password and create your admin user in the sudo group
..* adduser gemayel sudo 
4. Re-login as newly created admin user
5. Run commands to update your server
..* sudo apt-get update
..* sudo apt-get upgrade
..* sudo apt-get update (one more time)
####Setup Apache