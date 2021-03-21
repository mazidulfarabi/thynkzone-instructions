# humanitarian-instructions

Steps to follow to setup the humanitarian website & app

Check the humanitarian private repository for resources

- Host OS: Linux - Centos 7/8 / Ubuntu or Windows - Server / XP / 8 / 10
- Best choice: Centos 7
- Minimum Storage - 1 Gb
- Minimum Ram - 500 Mb
- Must have a static Public IP Address (make static if variable)
- Must have ports 22,80,8080,3306,443 open

GCP help: https://youtu.be/fMqFxV_0-DQ
AWS help: https://youtu.be/PrkEulPOV4s
Oracle Cloud help: https://youtu.be/6dkMo29HyM0

1) Connect to SSH via Browser or PUTTY (mostly - port 22)
- sudo passwd and create root user password
- su to login as root

2) Memory Swap if minimum storage >= 4 Gb
- follow exactly https://thinkersbase.blogspot.in/2018/03/create-linux-swap.html or might run out of storage
- check to make sure it worked

4) install mysql (el7-5)
- follow https://cloud.google.com/solutions/setup-mysql (set remote login disabled to no)
- set root password
- create user Farabi with password from the secure folder eu.txt
- flush previlages
- create database thynkzone
- USE thynkzone; copy parts of code (tables etc) from thynkzone.sql
- create database users
- check if mysql running

5) install java (openjdk-14's latest)
- follow https://youtu.be/90-0dRxs1fs (centos7) or https://youtu.be/u-6s7osqRvY (ubuntu) rather install "openjdk-14"; follow until the end to set bash .src path
- check java -version

6) install tomcat (9's latest)
- follow https://youtu.be/qgUIA8EwkB0 and install in /usr/local/tomcat9 don't skip the grep | java part
- follow till the end to set users and passwords as well but change username and password fields
- increase heap storage to minimum 128 Mb https://stackoverflow.com/questions/2718786/how-to-increase-java-heap-space-for-a-tomcat-app - see Aniket Thakur's answer below
- check in browser (ip address if domain n'yet dns pointed and :8080 if ports not configured in server.xml) if tomcat running & if not; then check if ports are open

7) install SSL (cloudflare)
- register domain, open account at cloudflare.com and enter that domain
- make sure nameservers are right, then setup dns, ssl, speed and other settings
- get origin certificate and set security to strict at ssl page
- create .crt and .pem in tomcat9/conf folder and paste the origin cert's pubklic crt and private key pem
- see the image in humanitarian private repository and edit server.xml following that
- restart tomcat server and wait for a few minutes to see if worked

8) deploy war (from private repository)
- go to domain/manager/html/
- delete current ROOT and deploy the root from humanitarian private repository - root after ssl - deploy after renaming it to ROOT.war
- check if online

9) make sure mail reaches
- create account, if mail doesn't reach, create app password for thynkzone.help and paste at secured folder ep.txt and check mailer.java and reupload ROOT.war

9) backup database
- ter two months, backup database using mysqldump

10) android app changes
-  publish domain or other changes for the android app, login to kodular using google sign in as detectivemailoffical
-  set version += 1 sub-version += 1
-  export as AAB with name Humanitarian
-  login to google play as thynkzone and create release and upload and then publish
