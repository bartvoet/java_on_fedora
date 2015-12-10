## Installing Git

We start of with installing git, this will allow to pull the examples.

~~~    
    # dnf install git
    $ git --version
    git version 2.4.3
~~~

> Don't worry, we will only use this to fetch some of the examples

## Downloading the code-samples

~~~    
    $ cd
    $ git clone https://github.com/bartvoet/java_on_fedora.git
    $ git clone https://github.com/heroku/java-sample.git
~~~

## Installing Java

When developing java you need a JDK, in most of the caseS you're confronted with 2 alternatives :

* The openjdk-version, open source and available for installation over dnf
* The Oracle JDK, a binary version (gratis but not free).  
  Basically based on the openjdk, with some closed source extensions

As quoted from stack-echange by Oracle JDK, the main differences between both are ...

> It is very close - our build process for Oracle JDK releases builds on OpenJDK 7 by adding just a couple of pieces, 
> like the deployment code, which includes Oracle's implementation of the Java Plugin and Java WebStart, 
> as well as some closed source third party components like a graphics rasterizer, some open source third party components, like Rhino, and a few bits and pieces here and there, like additional documentation or third party fonts.  
> Moving forward, our intent is to open source all pieces of the Oracle JDK except those that we consider commercial features such as JRockit Mission Control (not yet available in Oracle JDK), and replace encumbered third party components with open source alternatives to achieve closer parity between the code bases.  
> See also the following discussion on  [stackoverflow](http://stackoverflow.com/questions/22358071/differences-between-oracle-jdk-and-open-jdk-and-garbage-collection)

In this course we install both (concurrently) and show (so that you have the choice later depending on the needs of nature of your java-projects) you can configure these.

### Installing the open JDK

Installing the open-JDK is (very simple) to be installed via the following dnf-command 

~~~
    # dnf install java-1.8.0-openjdk-devel
    # ls -lh /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.65-3.b17.fc22.i386/
    totaal 20K
    drwxr-xr-x. 2 root root 4,0K 10 dec 09:04 bin
    drwxr-xr-x. 3 root root 4,0K 10 dec 09:04 include
    drwxr-xr-x. 4 root root 4,0K 10 dec 09:04 jre
    drwxr-xr-x. 3 root root 4,0K 10 dec 09:04 lib
    drwxr-xr-x. 2 root root 4,0K 10 dec 09:04 tapset
~~~

That's all you need to install openjdk, now let's install the second one.

### Installing the Oracle SDK

Installing the Oracle SDK requires some manual steps:

* Downloading the rpm-file manually  
 (or alternatively install from tar-file)
* Installing the local rpm-file 
* Cleaning up

~~~
    $ wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F;oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u65-b17/jdk-8u65-linux-i586.rpm"
    # rpm -Uvh jdk-8u65-linux-i586.rpm
    # ls -l /usr/java/jdk1.8.0_65
    totaal 25920
    drwxr-xr-x. 2 root root     4096  8 dec 19:07 bin
    -rw-r--r--. 1 root root     3244  7 okt 00:50 COPYRIGHT
    drwxr-xr-x. 4 root root     4096  8 dec 19:07 db
    drwxr-xr-x. 3 root root     4096  8 dec 19:07 include
    -rwxr-xr-x. 1 root root  5104144  6 okt 21:18 javafx-src.zip
    drwxr-xr-x. 5 root root     4096  8 dec 19:07 jre
    drwxr-xr-x. 5 root root     4096  8 dec 19:07 lib
    -rw-r--r--. 1 root root       40  7 okt 00:50 LICENSE
    drwxr-xr-x. 4 root root     4096  8 dec 19:07 man
    -rw-r--r--. 1 root root      159  7 okt 00:50 README.html
    -rw-r--r--. 1 root root      524  7 okt 00:50 release
    -rw-r--r--. 1 root root 21099412  7 okt 00:50 src.zip
    -rwxr-xr-x. 1 root root   110114  6 okt 21:18 THIRDPARTYLICENSEREADME-JAVAFX.txt
    -rw-r--r--. 1 root root   177094  7 okt 00:50 THIRDPARTYLICENSEREADME.txt
    $ rm ~/jdk-8u65-linux-i586.rpm
~~~   
    
### Selecting java and javac 

Next we need to configure which java-executable will be included in the path:
   
~~~    
    # alternatives --config java
~~~

You see an command-line-menu.
In this excercise we will choose the 2nd option:

~~~
    Er zijn 2 programma's die 'java' leveren.

      Selectie    Commando
    -----------------------------------------------
    *+ 1           /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.45-36.b13.fc22.i386/jre/bin/java
       2           /usr/java/jdk1.8.0_65/jre/bin/java
    
    <enter> om de huidige selectie te bewaren[+], of type een selectie nummer: 2


    # alternatives --config javac
    
    Er zijn 2 programma's die 'javac' leveren.
    
      Selectie    Commando
    -----------------------------------------------
       1           /usr/java/jdk1.8.0_65/bin/javac
    *+ 2           /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.65-3.b17.fc22.i386/bin/javac

    <enter> om de huidige selectie te bewaren[+], of type een selectie nummer: 1
~~~

You will see that /etc/alternatives has been changed.

~~~
    # ls -lh /etc/alternatives/*java*
    lrwxrwxrwx. 1 root root 34 10 dec 09:10 /etc/alternatives/java -> /usr/java/jdk1.8.0_65/jre/bin/java
    lrwxrwxrwx. 1 root root 37 10 dec 09:23 /etc/alternatives/java.1 -> /usr/java/jdk1.8.0_65/man/man1/java.1
    lrwxrwxrwx. 1 root root 31 10 dec 09:23 /etc/alternatives/javac -> /usr/java/jdk1.8.0_65/bin/javac
    lrwxrwxrwx. 1 root root 38 10 dec 09:23 /etc/alternatives/javac.1 -> /usr/java/jdk1.8.0_65/man/man1/javac.1
    lrwxrwxrwx. 1 root root 33 10 dec 09:23 /etc/alternatives/javadoc -> /usr/java/jdk1.8.0_65/bin/javadoc
    lrwxrwxrwx. 1 root root 40 10 dec 09:23 /etc/alternatives/javadoc.1 -> /usr/java/jdk1.8.0_65/man/man1/javadoc.1
    lrwxrwxrwx. 1 root root 40 10 dec 09:23 /etc/alternatives/javafxpackager -> /usr/java/jdk1.8.0_65/bin/javafxpackager
    lrwxrwxrwx. 1 root root 47 10 dec 09:23 /etc/alternatives/javafxpackager.1 -> /usr/java/jdk1.8.0_65/man/man1/javafxpackager.1
    lrwxrwxrwx. 1 root root 31 10 dec 09:23 /etc/alternatives/javah -> /usr/java/jdk1.8.0_65/bin/javah
    lrwxrwxrwx. 1 root root 38 10 dec 09:23 /etc/alternatives/javah.1 -> /usr/java/jdk1.8.0_65/man/man1/javah.1
    lrwxrwxrwx. 1 root root 31 10 dec 09:23 /etc/alternatives/javap -> /usr/java/jdk1.8.0_65/bin/javap
    lrwxrwxrwx. 1 root root 38 10 dec 09:23 /etc/alternatives/javap.1 -> /usr/java/jdk1.8.0_65/man/man1/javap.1
    lrwxrwxrwx. 1 root root 38 10 dec 09:23 /etc/alternatives/javapackager -> /usr/java/jdk1.8.0_65/bin/javapackager
    lrwxrwxrwx. 1 root root 45 10 dec 09:23 /etc/alternatives/javapackager.1 -> /usr/java/jdk1.8.0_65/man/man1/javapackager.1
    lrwxrwxrwx. 1 root root 38 10 dec 09:23 /etc/alternatives/java-rmi.cgi -> /usr/java/jdk1.8.0_65/bin/java-rmi.cgi
    lrwxrwxrwx. 1 root root 56 10 dec 09:04 /etc/alternatives/java_sdk_1.8.0 -> /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.65-3.b17.fc22.i386
    lrwxrwxrwx. 1 root root 64 10 dec 09:04 /etc/alternatives/java_sdk_1.8.0_exports -> /usr/lib/jvm-exports/java-1.8.0-openjdk-1.8.0.65-3.b17.fc22.i386
    lrwxrwxrwx. 1 root root 56 10 dec 09:04 /etc/alternatives/java_sdk_1.8.0_openjdk -> /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.65-3.b17.fc22.i386
    lrwxrwxrwx. 1 root root 64 10 dec 09:04 /etc/alternatives/java_sdk_1.8.0_openjdk_exports -> /usr/lib/jvm-exports/java-1.8.0-openjdk-1.8.0.65-3.b17.fc22.i386
    lrwxrwxrwx. 1 root root 56 10 dec 09:04 /etc/alternatives/java_sdk_openjdk -> /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.65-3.b17.fc22.i386
    lrwxrwxrwx. 1 root root 64 10 dec 09:04 /etc/alternatives/java_sdk_openjdk_exports -> /usr/lib/jvm-exports/java-1.8.0-openjdk-1.8.0.65-3.b17.fc22.i386
    lrwxrwxrwx. 1 root root 32 10 dec 09:23 /etc/alternatives/javaws -> /usr/java/jdk1.8.0_65/bin/javaws
    lrwxrwxrwx. 1 root root 39 10 dec 09:23 /etc/alternatives/javaws.1 -> /usr/java/jdk1.8.0_65/man/man1/javaws.1
~~~

### Selecting java and javac

After that you make sure that you create the (necessary) environment variable (JAVA_HOME).

For that we create a new file in the profile.d-directory

~~~    
    # vi /etc/profile.d/java.sh
~~~

And insert the following (using the location we observed via the alternatives-command. 

~~~
    export JAVA_HOME=/usr/java/jdk1.8.0_60/
    # optional since alternatives is configured
    # export PATH=$JAVA_HOME/bin:$PATH
~~~


## Testing your java-configuration (hello world)

~~~
    $ java -version
    java version "1.8.0_65"
    Java(TM) SE Runtime Environment (build 1.8.0_65-b17)
    Java HotSpot(TM) Client VM (build 25.65-b01, mixed mode, sharing)
~~~

Create a java-file with vi

~~~
   $ vi HelloWorld.java
~~~

Containing a simple hello-world, e.g. the following content 

~~~{.java }
    public class HelloWorld {
        public static void main(String[] args) {
            System.out.println("Hello world");
        }
    }
~~~

Compile it:

~~~
    javac HelloWorld.java
~~~
 
### Configuring classpath (jar-files)

Create the following java-file:

~~~{.java} 
    public class HelloWorldExtended {
            public static void main(String[] args) {
                    Hello hello = new Hello();
                    World world = new World();
                    hello.saySomething();
                    System.out.print();
                    world.saySomething();
                    System.out.println();
            }
    }
~~~

Compiling this file will result in an error because of other depending classes not in the **classpath**
 
~~~
    $ javac HelloWorld.java 
    HelloWorld.java:3: error: cannot find symbol
    		Hello hello = new Hello();
    		^
      symbol:   class Hello
      location: class HelloWorld
    HelloWorld.java:3: error: cannot find symbol
    		Hello hello = new Hello();
    		                  ^
      symbol:   class Hello
      location: class HelloWorld
    HelloWorld.java:4: error: cannot find symbol
    		World world = new World();
    		^
      symbol:   class World
      location: class HelloWorld
    HelloWorld.java:4: error: cannot find symbol
    		World world = new World();
    		                  ^
      symbol:   class World
      location: class HelloWorld
    4 errors
~~~
 
You create a separate folder hello and create a java-file "Hello.java"

~~~
    mkdir hello
    vi hello/Hello.java
    javac hello/Hello.java
~~~

with e.g. content

~~~{.java} 
    public class Hello {
            public void saySomething() {
                    System.out.print("Hello");
            }
    }
~~~

and a seconde folder world

~~~
    mkdir world
    vi world/World.java
    javac world/World.java
~~~

with the content

~~~{.java} 
    public class World {
            public void saySomething() {
                    System.out.print("World");
            }
    }
~~~

Don't forget to include in the classpath a dot (only required for executing)

~~~
    $ javac -classpath ./hello:./world HelloWorld.java
    $ java -classpath ./hello:./world:. HelloWorld
    Hello World
~~~
 
> Note:  
> Pay attention for the difference with Windows where the different elements of the classpath are separated by 
> a ';' rather than a ':' 
 
### Testing with jar-files

~~~
    $ cd hello
    $ jar -cf hello.jar Hello.class
    $ cd ../world
    $ jar -cf world.jar World.class
    $ cd ..
    $ javac -classpath ./hello/hello.jar:./world/world.jar HelloWorld.java
    $ java -classpath ./hello/hello.jar:./world/world.jar:. HelloWorld
    Hello World
~~~
  
## Installing Eclipse

Eclipse is the most widely used java-IDE.  
Problem with the most Linux-distribution is that they lack behind with the version of eclipse.  
Now that you've installed java however the installation of eclipse without package manager is very easy.  

~~~
    wget http://ftp-stud.fht-esslingen.de/pub/Mirrors/eclipse/technology/epp/downloads/release/mars/1/eclipse-jee-mars-1-linux-gtk.tar.gz
    tar -zxvf eclipse-jee-mars-1-linux-gtk.tar.gz
    rm eclipse-jee-mars-1-linux-gtk.tar.gz
    ./eclipse/eclipse
~~~

After that you can ...

~~~
vi ./eclipse/eclipse.ini
~~~

lwhere you can configure the minimum and maximum heaps size
e.g.


## Installing Tomcat

We will install Tomcat from the dnf-repository (althought this also possible like eclipse to install from the ...)

~~~
    # dnf install tomcat
    # vi /usr/share/tomcat/conf/tomcat.conf
    JAVA_OPTS="-Djava.security.egd=file:/dev/./urandom -Djava.awt.headless=true -Xmx512m -XX:MaxPermSize=256m -XX:+UseConcMarkSweepGC"
    # dnf install tomcat-webapps tomcat-admin-webapps
    # dnf install tomcat-docs-webapp tomcat-javadoc
~~~

Next step is to configure locally a password so that you can configure

~~~
    # vi /usr/share/tomcat/conf/tomcat-users.xml
~~~

~~~{.xml}
    <tomcat-users>
        <user username="admin" password="password" roles="manager-gui,admin-gui"/>
    </tomcat-users>
~~~

http://localhost:8080

## Install Maven
    
Next step is installing a web-application.  
We will use maven for building this application.  
Download for this the most recent maven release and untar it to the opt-folder.

~~~
    $ wget http://mirrors.gigenet.com/apache/maven/maven-3/3.0.5/binaries/apache-maven-3.0.5-bin.tar.gz
    # tar -zxvf apache-maven-3.0.5-bin.tar.gz -C /opt/
~~~

Maven is to be placed in the path so we create a profile-script:

~~~
    # vi /etc/profile.d/maven.sh
~~~

With the content

~~~{.sh}
    export M2_HOME=/opt/apache-maven-3.3.9
    export M2=$M2_HOME/bin
    PATH=$M2:$PATH
~~~

## Checkout and compile

Once this is done we:

* Use git in order to checkout a sample web-project
* Compile it via maven (and the jdk we've set up earlier)
* 

~~~
    $ git clone https://github.com/heroku/java-sample.git
    $ cd java-sample
    $ mvn clean all
    
    [bart@localhost java-sample]$ maven clean package
    bash: maven: opdracht niet gevonden
    [bart@localhost java-sample]$ mvn clean package
    [INFO] Scanning for projects...
    [INFO]                                                                         
    [INFO] ------------------------------------------------------------------------
    [INFO] Building webappRunnerSample Maven Webapp 1.0-SNAPSHOT
    [INFO] ------------------------------------------------------------------------
    [INFO] 
    [INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ webappRunnerSample 

...

    [INFO] Deleting /home/bart/java-sample/target
    [INFO] 
    [INFO] --- maven-resources-plugin:2.6:resources (default-resources) @  ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 6.034 s
    [INFO] Finished at: 2015-12-10T08:13:07+01:00
    [INFO] Final Memory: 10M/23M
    [INFO] ------------------------------------------------------------------------
~~~

Via the webconsole
See [Tomcat Documentation](https://tomcat.apache.org/tomcat-7.0-doc/deployer-howto.html)

~~~
# webapps_dir=/var/lib/tomcat/webapps
# cp webappRunnerSample.war $webapps_dir
# systemctl restart tomcat
~~~



Testing can be done through the webbrowser and a simple curl:

~~~
    curl http://localhost:8080/webappRunnerSample/
    <html>
    <body>
    <h2>Hello World!</h2>
    </body>
    </html>
~~~

    