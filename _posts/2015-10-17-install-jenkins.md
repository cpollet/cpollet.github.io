---
layout: post
title: Install Jenkins
---

After installation of Ubuntu 14.04 LTS, as root (just like every other commands here):

 * $ locale-gen en_US en_US.UTF-8 fr_CH fr_CH.UTF-8
 * $ dpkg-reconfigure locales
 
# Install Java
* Get Java from [http://www.oracle.com/technetwork/java/javase/downloads/index.html](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Extract it in `/opt`
* Create symlinks if you wish `$ ln -s jdk1.8.0_60 java-1.8`
* Create symlinks if you wish `$ ln -s java-1.8 java`

# Install Maven
* Get maven from [https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi)
* Extract it in `/opt`
* Create symlinks if you wish 
* `$ ln -s apache-maven-3.3.3 maven`

# Install Jenkins
* `$ wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | apt-key add -`
* `$ sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'`
* `$ apt-get update`
* `$ apt-get install jenkins`
* `$ apt-get install apache2`
* `$ a2enmod proxy`
* `$ a2enmod http_proxy`
* Create `/etc/apache2/sites-available/jenkins.conf` with content below
* `$ a2ensite jenkins`
* `$ apache2ctl restart`

#### Virtual host file

    <VirtualHost *:80>
        ServerAdmin webmaster@localhost
        ServerName ci.company.com
        ServerAlias ci
        ProxyRequests Off
        <Proxy *>
            Order deny,allow
            Allow from all
        </Proxy>
        ProxyPreserveHost on
        ProxyPass / http://localhost:8080/ nocanon
        AllowEncodedSlashes NoDecode
    </VirtualHost>

# Install mysql
* `$ apt-get install mysql-server`

# Install sonar
* Get sonar from [http://www.sonarsource.org/downloads/](http://www.sonarsource.org/downloads/)
* Extract in `/opt`
* `$ ln -s sonarqube-5.1.2 sonar`
* `$ ln -s /opt/sonar/bin/linux-x86-64/sonar.sh /usr/bin/sonar`
* `$ mysql -u root -p`
* `mysql> CREATE DATABASE IF NOT EXISTS sonar CHARACTER SET utf8 COLLATE utf8_bin;`
* `mysql> CREATE USER 'sonar'@'%' IDENTIFIED BY 'sonar';` here you may use a different password...
* `mysql> GRANT ALL ON sonar.* TO 'sonar'@'%';`
* `mysql> quit;`
* Configure sonar (edit `/opt/sonar/conf/sonar.properties`):
    * Search for "Embedded database Derby" and comment everything
    * Search for mysql section and uncomment it
* Configure maven settings (`/opt/maven/conf/settings.xml`) with content below in the `<profiles>` section 
* Create `/etc/init.d/sonar` with content below
* `$ chmod 755 /etc/init.d/sonar`
* `$ update-rc.d sonar defaults 98 02`
* You can use `/etc/init.d/sonar start` to start sonar
* You can create a virtual host with the same technique as for jenkins

#### Content of `/etc/init.d/sonar`

    #!/bin/sh
    /usr/bin/sonar $*

#### Sonar profile in `/opt/maven/conf/settings.xml`

    <profile>
        <id>sonar</id>
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
        <properties>
            <sonar.jdbc.url>
              jdbc:mysql://localhost:3306/sonar?useUnicode=true&amp;characterEncoding=utf8
            </sonar.jdbc.url>
            <sonar.jdbc.username>sonar</sonar.jdbc.username>
            <sonar.jdbc.password>sonar</sonar.jdbc.password>
    
            <!-- Optional URL to server. Default value is http://localhost:9000 -->
            <sonar.host.url>
              http://myserver:9000
            </sonar.host.url>
        </properties>
    </profile>

<i class="fa fa-exclamation-triangle fa-lg"></i> make sure you have jacoco 0.7.5.201505241946 and sonar's Java plugin >= 3.4 otherwise it wont work.
You'll get something like

    [ERROR] Failed to execute goal org.codehaus.mojo:sonar-maven-plugin:2.6:sonar (default-cli) on project ...: Unable to read .../target/jacoco.exec: Incompatible version 1007. -> [Help 1]

If you followed all the steps, you should be all set! It's time to trigger a build

#### Resources

* [http://ubuntuforums.org/showthread.php?t=1346581](http://ubuntuforums.org/showthread.php?t=1346581)
* [https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu)
* [http://doc.ubuntu-fr.org/sonar](http://doc.ubuntu-fr.org/sonar)
* [http://docs.sonarqube.org/display/SONAR/Installing+and+Configuring+Maven](http://docs.sonarqube.org/display/SONAR/Installing+and+Configuring+Maven)
* [http://doc.ubuntu-fr.org/mysql](http://doc.ubuntu-fr.org/mysql)