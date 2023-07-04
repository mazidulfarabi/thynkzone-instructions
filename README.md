# Thynkzone-instructions

Steps to follow to setup the Thynkzone website & app

Current Thynkzone dynamic app & website: https://thynkzone.eu.org
Google Play: https://play.google.com/store/apps/details?id=io.kodular.detectivemailofficial.Thynkzone
- Currently hosted at: Oracle Free Tier, Ubuntu 22.04, Ampere Arm Processor 4 OCPU, 24 GB memory, 200GB disk volume, OpenJdk 18, mySQL 8, tomcat 9
- Used languages: Java EE, Javascript, Jquery(Ajax), MySQL, HTML, CSS
- Used ide: Eclipse Java EE
- Used server: tomcat9
- (For setup) See Thynkzone private repository to find the ROOT.war & Thynkzone.aab (for Google Play) files.
- This is a developer-instructions readme.

Check the Thynkzone private repository for resources

- Host OS: Linux - Ubuntu 22.04 or Centos 7/8 or Windows - Server / XP / 8 / 10
- Recommended: Ubuntu 22.04
- Required Minimum Storage - 1 Gb
- Required Minimum Ram - 500 Mb
- Must have a static Public IP Address (make static/elastic if variable/dynamic)
- Must have ports 22,80,8080,3306,443 open on TCP

GCP help: https://youtu.be/fMqFxV_0-DQ

AWS help: https://youtu.be/PrkEulPOV4s

Oracle Cloud help: https://youtu.be/6dkMo29HyM0

-----------------------------------------------------------------
Thynkzone WEBSITE AND APP ~ SETUP AND DEPLOYMENT INSTRUCTIONS
-----------------------------------------------------------------

0) IDE setup eclipse, vscode & notepad++ for local editing and debugging

Step 1 - Eclipse (required)
- download and install xampp server for phpmyadmin and import thynkzone.sql database after creating, and create users database
- in phpmyadmin, create new user farabi, set password from secured, grant previlages and flush
- download and install java - openjdk15
- download and install eclipse-jee latest
- download tomcat 9's latest
- rename tomcat to tomcat9
- add tomcat server in eclipse
- in eclipse, select file -> import -> web -> war
- import thynkzone private repo's ROOT - after ssl.war after renaming it to thynkzone.war & disselect jars (default) at next
- similarly import thynkzone2.war
- run as server & add the projects to the server
- make sure to refresh browser cache if css updates not working
- add project to github repository, following https://youtu.be/LPT7v69guVY
- to make ready for hosted in other servers, make sure 2 things in following and do vice versa for ROOT.war to local
----------------------------------------------------------------------------------------------------------------------

- delete all extra images and img from eclipse, also delete all users tables and thynkzone table entries from database if ROOT is not duplicated for future home-development
- set cookies to secure in loginuser3.jsp and db.java; change filepaths and '\\' to '/' at following
- af,bf,cf,ss,db(twice),move2,move3,move4(twice),move5,postpro,postproedit,process2,adpro,bgprocess
>>Change filepath to -> /usr/local/tomcat9/webapps/ROOT/WEB-INF/secured/x.txt (x/y/z/only secured - depending on one at place);
>>In db,move,etc. (for urls that end in /img/), set imagepath to /usr/local/img/ and
>>In postpro,etc. (for urls that end in /images) set: /usr/local/tomcat9/webapps/ROOT/images
- export project as ROOT.war

>>Domain Change --> Google reCaptcha at Google reCaptcha website only, (thynkzone.help gmail account), Google Play Console app privacy policy url, Disqus Comments link at Disqus website only, and ssl-certificates at cloudflare and tomcat/conf/...cert and ...key (and maybe also ...ca.bundle)

Step 2 - VS Code (optional)
note: currently no live web browser view support and war export support
- complete all steps in step 1
- download and install vscode
- install java essential extensions pack from, https://code.visualstudio.com/docs/languages/java
- install tomcat server adding option to vscode at, https://marketplace.visualstudio.com/items?itemName=adashen.vscode-tomcat
- add tomcat server from eclipse/tomcat9
- add github integration, following https://youtu.be/3Tn58KQvWtU
- go to source control -> open folder containing a repository
- remeber to push changes after commit

Step 3 - Notepad++ (optional but very useful)
- use for find/replace in all pages at once



----------------------------------------------------------------------------------------------VM/VPS Installations steps

1) Connect to SSH via browser or PUTTY (mostly - port 22)
- sudo passwd and create root user password
- su to login as root

2) Memory swap if minimum storage >= 4 Gb -- (Optional Step)
- follow exactly https://thinkersbase.blogspot.in/2018/03/create-linux-swap.html or might run out of storage
- check to make sure it worked

3) Install mysql
- follow https://cloud.google.com/solutions/setup-mysql  or https://youtu.be/TG6WAnyeDRw
- set root password
- create user Farabi with password from the secure folder ep.txt+"09"
- grant all permissions
- flush previlages
- create database thynkzone
- USE thynkzone; copy code from thynkzone.sql at the thynkzone private repository
- create database users
- increase max connections -> SET GLOBAL -> restart mysql (service mysql restart)
- check if mysql running

4) Install java (oraclejdk-18's latest)
- follow https://youtu.be/90-0dRxs1fs (centos7) or https://youtu.be/u-6s7osqRvY (ubuntu) rather install "openjdk-18"; follow until the end to set bash .src path
- check java -version
- check echo $JAVA_HOME

5) Install tomcat (9's latest)
- follow https://youtu.be/qgUIA8EwkB0 and install in /usr/local/tomcat9 don't skip the grep java part
- follow till the end to set users and passwords as well but change username and password fields
- increase heap storage to minimum 128 Mb https://stackoverflow.com/questions/2718786/how-to-increase-java-heap-space-for-a-tomcat-app - see Aniket Thakur's answer below -- (Optional Step)
- in server.xml, change max thread size and max connections
- in sever.xml, create docbase for path img in root, so that upon ROOT.war redeploy, image files aren't lost -> create img folder in /usr/local/ and copy prof.png, blank.webp by following command
- cp /usr/local/tomcat9/webapps/ROOT/img/prof.png /usr/local/img/prof.png (see ssl-server-xml-edit pics in thynkzone private repo) - or wget
- mv /usr/local/tomcat9/webapps/ROOT/img/prof.png and /usr/local/img/blank.webp - wget from any image hosting service
- check in browser (use ip address if domain n'yet dns pointed and :8080 or /login.jsp if 443 not configured in server.xml) if tomcat running & if not; then check if ports are open in vm network ports
- make sure in server.xml, port 443 uses -- http11nioprotocol

6) Install SSL (cloudflare)
- register domain, open account at cloudflare.com and enter that domain
- make sure nameservers are right, then setup dns, ssl, speed and other settings
- get origin certificate and set security to strict at ssl page
- create .crt and .key and ca_bundle.crt in tomcat9/conf folder and paste the origin cert's pubklic crt and private key pem
- edit server.xml (see ssl-server-xml-edit pics in thynkzone private repo and add admin-gui and make allowipaddress.* or uncomment that in valve settings in somefile.xml)
- edit conf/tomcat-users.xml
- restart tomcat server and wait for a few minutes to see if worked

7) Deploy war (from private repository)
- go to domain/manager/html/
- check for manager password in private repo ss OR login as sudo -s and navigate to /usr/local/tomcat9/conf/server.xml and see manager password
- stop and undeploy current ROOT and deploy the root from thynkzone private repository - root (Latest) - deploy after renaming it to ROOT.war
- check if online

8) Make sure mail reaches
- create account, if mail doesn't reach, create app password for thynkzone.help and paste at secured folder ep.txt and check mailer.java and reupload ROOT.war

9) Backup images and database
- per two months, backup database using mysqldump and download (https://stackoverflow.com/questions/9427553/how-to-download-a-file-from-server-using-ssh)
- backup by downloading all images from /usr/local/img and download (https://stackoverflow.com/questions/9427553/how-to-download-a-file-from-server-using-ssh)

10) Android app changes
-  publish domain or other changes for the android app, login to kodular using google sign in as detectivemailoffical
-  set version += 1 sub-version += 1
-  export as AAB with name thynkzone
-  login to google play as thynkzone and create release and upload and then publish

P.S. Idea for phpmyadmin installation
- install apache and phpmyadmin following steps from youtube videos
- remember, when using phpmyadmin, port 80 needs to be open for apache server, so shutdown tomcat for accessing phpmyadmin and making changes
- assign a Under Construction page and name it index.html under apache server (htdocs probably) so that whenever tomcat is off, domain shows that page to users

P.S. kodular
- Add AdMob
