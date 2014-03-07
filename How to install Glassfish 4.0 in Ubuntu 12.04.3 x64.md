## How to install Oracle Java 7 and Glassfish 4.0 in Ubuntu 12.04.3 x64

There are many tutorials to install OpenJDK and JBoss. This is one on the latest Oracle Java and Glassfish. Hopefully this will make deploying easier for Java EE developers.
<h2>Pre-conditions</h2></br>
A droplet with Ubuntu 12.04.3 x64 has been created in DigitalOcean. Login as root by ssh. Assuming there's no Java installed before or you have uninstalled them.</br>For this tutorial the droplet has 1G memory, as Java EE servers are quite demanding.

<h2>What is Glassfish?</h2>
GlassFish is an open-source application server and the reference implementation of Java EE. GlassFish 4.0 release supports the latest Java Platform, Enterprise Edition 7. It supports Enterprise JavaBeans, JPA, JavaServer Faces, JMS, RMI, JavaServer Pages, servlets, etc.

<h2>Step One: Install Oracle Java 7</h2>
In order to get Oracle Installer of Java 7, we need to add new apt repository:
<pre># sudo add-apt-repository ppa:webupd8team/java</pre>

I happen to confront the following error:
<pre>sudo: add-apt-repository: command not found</pre>
The reason is the default installation of Ubuntu doesn't have the command installed. In order to use add-apt-repository, you need to install python-software-properties. Here's how to do it by apt-get:
<pre>
# sudo apt-get install python-software-properties
</pre>
Now you can add the new repository and install from Oracle Installer as the first step:
<pre>
# sudo add-apt-repository ppa:webupd8team/java</pre>
Make source list up-to-date:
<pre># sudo apt-get update</pre>
Install Java 7 by apt-get:
<pre># sudo apt-get install oracle-java7-installer</pre>

After installing, confirm the current Java is Oracle version:
<pre># java -version
java version "1.7.0_51"
Java(TM) SE Runtime Environment (build 1.7.0_51-b13)
Java HotSpot(TM) 64-Bit Server VM (build 24.51-b03, mixed mode)</pre>


<h2>Step Two: Install Glassfish 4.0</h2>
Get Glassfish Zip file
<pre>$ wget download.java.net/glassfish/4.0/release/glassfish-4.0.zip</pre>
Install unzip first before unpackage to /opt
<pre># apt-get install unzip</pre>
 Create the directory /opt, and then unzip the package to /opt:
<pre># unzip glassfish-4.0.zip </pre>

For convenience, add path to asadmin to PATH by adding below to the end of ~/.profile
<pre>export PATH=/opt/glassfish4/bin:$PATH</pre>

Start the glassfish server:
<pre>
# asadmin start-domain
Waiting for domain1 to start ...................
Successfully started the domain : domain1
domain  Location: /opt/glassfish4/glassfish/domains/domain1
Log File: /opt/glassfish4/glassfish/domains/domain1/logs/server.log
Admin Port: 4848
Command start-domain executed successfully.
</pre>
A domain is a set of one or more GlassFish Server instances managed by one administration server. 
Default GlassFish Server’s port number: 8080.
Default administration server’s port number: 4848.
Administration user name: admin; password: none.

In order to visit admin page (your_server_id:4848) remotely, you need to enable secure admin:
<pre># asadmin enable-secure-admin
Enter admin user name>  admin
Enter admin password for user "admin"> 
You must restart all running servers for the change in secure admin to take effect.
Command enable-secure-admin executed successfully.</pre>

Restart domain to make effect of secure admin:
<pre># asadmin restart-domain
Successfully restarted the domain
Command restart-domain executed successfully.</pre>

Now you can visit admin page (your_server_id:4848) in browser</br>

To stop the GlassFish server:
<pre># asadmin stop-domain
Waiting for the domain to stop .
Command stop-domain executed successfully.</pre>

<h2>Demo service: deploy hello.war on Glassfish</h2>
Download the sample application from Glassfish official samples:
<pre># wget https://glassfish.java.net/downloads/quickstart/hello.war</pre>
Deploy war file:
<pre># asadmin deploy /home/ee/glassfish/sample/hello.war
Enter admin user name>  admin
Enter admin password for user "admin"> 
Application deployed with name hello.
Command deploy executed successfully.</pre>

Now you can visit your_server_id:8080/hello</br>

To undeploy the application:
<pre># asadmin undeploy hello
Enter admin user name>  admin
Enter admin password for user "admin"> 
Command undeploy executed successfully.</pre>

In order to save typing admin user name and password every time you deploy or undeploy an application, create a password file pwdfile with content:
<pre>AS_ADMIN_PASSWORD=your_admin_password</pre>
Add --passwordfile in command:
<pre># asadmin --passwordfile pwdfile deploy /home/ee/glassfish/sample/hello.war</pre>
Now the prompt for user name/password won't appear.


