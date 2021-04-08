# humanitarian-instructions

Steps to follow to setup the humanitarian website & app

Current Humanitarian dynamic app & website: https://www.humanitarian.gq
Currently hosted at: AWS EC2 free tier, user: thynkzone
Used languages: Java, Javascript, Jquery(Ajax), MySQL, HTML, CSS
Used ide: Eclipse JEE
Used server: tomcat9
See humanitarian private repository to find the ROOT.war & Humanitarian.aab (for Google Play) files.
This is a developer-instructions readme.

Curremt user-instructions website: https://humanitarian.cf
Currently hosted at: 000webhost, shahriyarchi959
Used CMS: WordPress
See humanitarian private repository to find the WordPress export file.

Check the humanitarian private repository for resources

- Host OS: Linux - Centos 7/8 / Ubuntu or Windows - Server / XP / 8 / 10
- Best choice: Centos 7
- Minimum Storage - 1 Gb
- Minimum Ram - 500 Mb
- Must have a static Public IP Address (make static/elastic if variable)
- Must have ports 22,80,8080,3306,443 open

GCP help: https://youtu.be/fMqFxV_0-DQ

AWS help: https://youtu.be/PrkEulPOV4s

Oracle Cloud help: https://youtu.be/6dkMo29HyM0

-----------------------------------------------------------------
HUMANITARIAN WEBSITE AND APP ~ SETUP AND DEPLOYMENT INSTRUCTIONS
-----------------------------------------------------------------

0) IDE setup eclipse & vscode for local editing and debugging

Step 1 - Eclipse
- download and install xampp server for phpmyadmin and import thynkzone.sql database after creating, and create users database
- in phpmyadmin, create new user farabi, set password from secured, grant previlages and flush
- download and install java - openjdk15
- download and install eclipse-jee latest
- download tomcat 9's latest
- in eclipse, select file -> import -> web -> war
- import humanitarian private repo's ROOT - after ssl.war after renaming it to humanitarian.war
- similarly import humanitarian2.war
- add tomcat server in eclipse
- make sure to refresh browser cache if css updates not working
- add project to github repository, following https://youtu.be/LPT7v69guVY
- to make ready for hosted in other servers, make sure 2 things in following and do vice versa for ROOT.war to local
- set cookies to secure in loginuser3.jsp and db.java; change filepaths and '\\' to '/' at following
- af,bf,cf,ss,db,move2,move3,move4(twice),move5,postpro,postproedit,process2,adpro,bgprocess
- export project as ROOT.war

Step 2 - VS Code
- complete all steps in step 1
- download and install java vscode
- install java essential extensions pack from, https://code.visualstudio.com/docs/languages/java
- install tomcat server adding option to vscode at, https://marketplace.visualstudio.com/items?itemName=adashen.vscode-tomcat
- add tomcat server from eclipse/tomcat9
- click on open java project and select eclipse/projects/project_name
- add github integration, following https://youtu.be/3Tn58KQvWtU

1) Connect to SSH via browser or PUTTY (mostly - port 22)
- sudo passwd and create root user password
- su to login as root

2) Memory swap if minimum storage >= 4 Gb
- follow exactly https://thinkersbase.blogspot.in/2018/03/create-linux-swap.html or might run out of storage
- check to make sure it worked

3) Install mysql (el7-5)
- follow https://cloud.google.com/solutions/setup-mysql (set remote login disabled to no)
- set root password
- create user Farabi with password from the secure folder eu.txt
- grant permissions
- flush previlages
- create database thynkzone
- USE thynkzone; copy parts of code (tables etc) from thynkzone.sql at the humanitarian private repository
- create database users
- increase max connections
- check if mysql running

4) Install java (openjdk-14's latest)
- follow https://youtu.be/90-0dRxs1fs (centos7) or https://youtu.be/u-6s7osqRvY (ubuntu) rather install "openjdk-14"; follow until the end to set bash .src path
- check java -version

5) Install tomcat (9's latest)
- follow https://youtu.be/qgUIA8EwkB0 and install in /usr/local/tomcat9 don't skip the grep | java part
- follow till the end to set users and passwords as well but change username and password fields
- increase heap storage to minimum 128 Mb https://stackoverflow.com/questions/2718786/how-to-increase-java-heap-space-for-a-tomcat-app - see Aniket Thakur's answer below
- in server.xml, change max thread size and max connections
- in sever.xml, create docbase for path img in root, so that upon ROOT.war redeploy, image files aren't lost
- check in browser (use ip address if domain n'yet dns pointed and :8080 or /login.jsp if 443 not configured in server.xml) if tomcat running & if not; then check if ports are open in vm network ports

6) Install SSL (cloudflare)
- register domain, open account at cloudflare.com and enter that domain
- make sure nameservers are right, then setup dns, ssl, speed and other settings
- get origin certificate and set security to strict at ssl page
- create .crt and .pem in tomcat9/conf folder and paste the origin cert's pubklic crt and private key pem
- see the image file in humanitarian private repository and edit server.xml following that
- restart tomcat server and wait for a few minutes to see if worked

7) Deploy war (from private repository)
- go to domain/manager/html/
- delete current ROOT and deploy the root from humanitarian private repository - root after ssl - deploy after renaming it to ROOT.war
- check if online

8) Make sure mail reaches
- create account, if mail doesn't reach, create app password for thynkzone.help and paste at secured folder ep.txt and check mailer.java and reupload ROOT.war

9) Backup images and database
- per two months, backup database using mysqldump
- backup by downloading all images

10) Android app changes
-  publish domain or other changes for the android app, login to kodular using google sign in as detectivemailoffical
-  set version += 1 sub-version += 1
-  export as AAB with name Humanitarian
-  login to google play as thynkzone and create release and upload and then publish
