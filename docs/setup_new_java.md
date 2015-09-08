# Install Java

Go to the...
* prev section [Setup github](setup_new_github.md)
* new [Setup](setup_new.md) instructions
* next section [Install Leiningen](setup_new_lein.md)

## Download and install the JDK

For developing Java (and thus Clojure) programs you will
need the JDK (Java Development Kit -- includes extra tools)
vs. the JRE (Java Runtime Environment).

On Mac and Windows you can download JDK 8 from the Oracle website:

http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

On Linux you can use for favorite package manager (e.g. apt, synaptic,
rpm) to install the relevant packages:

````
# apt-get install openjdk-8-jdk
````

## Verify that Java is installed

In your command terminal you can verify that java is installed
by checking the version like this:

````
clojurista@mylaptop $ java -version
openjdk version "1.8.0_66-internal"
OpenJDK Runtime Environment (build 1.8.0_66-internal-b01)
OpenJDK 64-Bit Server VM (build 25.66-b01, mixed mode)
clojurista@mylaptop $
````
