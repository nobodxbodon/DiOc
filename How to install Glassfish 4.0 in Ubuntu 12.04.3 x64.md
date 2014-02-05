## How to install Oracle Java 7 and Glassfish 4.0 in Ubuntu 12.04.3 x64

Based on <a href="http://www.silvatechsolutions.com/tech-tips/glassfish-ubuntu-12-04-ubber-quick-install-guide/">Glassfish Ubuntu 12.04 Ubber Quick Install Guide</a>
<h2>Pre-conditions</h2></br>
A droplet with Ubuntu 12.04.3 x64 has been created in DigitalOcean. Login as root by ssh.

<h2>Step One—Install Oracle Java 7</h2>
Reference <a href="http://www.ubuntugeek.com/how-to-install-oracle-java-7-in-ubuntu-12-04.html">here</a>
<pre># sudo add-apt-repository ppa:webupd8team/java</pre>


When you get the following error 
<pre>sudo: add-apt-repository: command not found</pre>
You have to install software-properties-common according to <a href="http://linuxg.net/how-to-fix-error-sudo-add-apt-repository-command-not-found/">here</a>
<pre>
# sudo apt-get install software-properties-common
# sudo apt-get install python-software-properties
</pre>
Now you can add repository and install
<pre>
# sudo add-apt-repository ppa:webupd8team/java
# sudo apt-get update
# sudo apt-get install oracle-java7-installer</pre>

After installing
<pre># java -version
java version "1.7.0_51"
Java(TM) SE Runtime Environment (build 1.7.0_51-b13)
Java HotSpot(TM) 64-Bit Server VM (build 24.51-b03, mixed mode)</pre>


<h2>Step Two—Install Glassfish 4.0</h2>
Get Glassfish Zip file
<pre>$ wget download.java.net/glassfish/4.0/release/glassfish-4.0.zip</pre>
Install unzip first before unpackage to /opt
<pre># apt-get install unzip
# unzip glassfish-4.0.zip </pre>

Start the default domain
<pre>
/opt# glassfish4/bin/asadmin start-domain
Waiting for domain1 to start ...................
Successfully started the domain : domain1
domain  Location: /opt/glassfish4/glassfish/domains/domain1
Log File: /opt/glassfish4/glassfish/domains/domain1/logs/server.log
Admin Port: 4848
Command start-domain executed successfully.
</pre>

Now you can deploy war files as <a href="http://blog.c2b2.co.uk/2013/06/getting-started-with-glassfish-4.html">here</a>

