## Continuous Integration for Arduino with JenkinsCI 

This repository contains

- an ANT build file that can be used to build Arduino sketches.
- a Jenkins job template that can be used to setup a Jenkins job for your Arduino project.

## Preparing your source code

The build currently expects that the ino file contains the following imports and methods

	#include "Arduino.h"
	#include "configuration.h"
	void setup();
	void loop(); 

These need to be present at the top of the file. One of the things that the build file will do is copy your ```ino``` file into a standard ```cpp``` file. In order for the cpp compiler to work properly this needs to be in your source-code.

## Preparing your build environment

Your build environment (where you will be executing ANT / running the Jenkins job) needs to have the Arduino software installed.
You'll need to specify that location in the ```build.properties``` file.

For MacOSX this is most likely

	arduino-home=/Applications/Arduino.app/Contents/Resources/Java

For a linux based system this can be anything. Ony my setup it is located here:

	arduino-home=/usr/local/arduino-1.0.5


## Invoking the build

When invoking the ```ant compile``` you should get the following output:

	Buildfile: /Users/ddewaele/Projects/Arduino/Blink/build.xml

	clean:

	dist-name:
	     [echo] Building distribution : embedded-2-5.bin

	compile:
	    [mkdir] Created dir: /Users/ddewaele/Projects/Arduino/Blink/build/bin
	     [copy] Copied 5 empty directories to 5 empty directories under /Users/ddewaele/Projects/Arduino/Blink/build/bin
	     [copy] Copying 1 file to /Users/ddewaele/Projects/Arduino/Blink/build/bin
	     [echo] Copying libArduinoCore.a to /Users/ddewaele/Projects/Arduino/Blink/build/bin
	     [copy] Copying 1 file to /Users/ddewaele/Projects/Arduino/Blink/build/bin
	    [mkdir] Created dir: /Users/ddewaele/Projects/Arduino/Blink/build/bin/binaries
	     [copy] Copying 1 file to /usr/local/sketches

	BUILD SUCCESSFUL
	Total time: 1 second


You can also export a major and a minor version that will be picked up by the build:

	export majorVersion=2
	export minorVersion=5


By executing the ```ant compile``` command you'll get the following output:

	Buildfile: /Users/ddewaele/Projects/Arduino/Blink/build.xml

	clean:
	   [delete] Deleting directory /Users/ddewaele/Projects/Arduino/Blink/build

	dist-name:
	     [echo] Building distribution : embedded-2-5.bin

	compile:
	    [mkdir] Created dir: /Users/ddewaele/Projects/Arduino/Blink/build/bin
	     [copy] Copied 5 empty directories to 5 empty directories under /Users/ddewaele/Projects/Arduino/Blink/build/bin
	     [copy] Copying 1 file to /Users/ddewaele/Projects/Arduino/Blink/build/bin
	     [echo] Copying libArduinoCore.a to /Users/ddewaele/Projects/Arduino/Blink/build/bin
	     [copy] Copying 1 file to /Users/ddewaele/Projects/Arduino/Blink/build/bin
	    [mkdir] Created dir: /Users/ddewaele/Projects/Arduino/Blink/build/bin/binaries
	     [copy] Copying 1 file to /usr/local/sketches

	BUILD SUCCESSFUL
	Total time: 1 second


## Embedding the build in a Jenkins Continuous Integration Environment

You can easily embed this build in a a Jenkins Continuous Integration Environment

### Getting the hudson CLI jar

First thing you'll be needing is the ```hudson-cli.jar``` file. You can download it from your jenkins installation like this.

	wget http://jenkins.mycompany.com/jnlpJars/hudson-cli.jar

### Creating a Jenkins job with the CLI

	java -jar hudson-cli.jar -s http://jenkins.mycompany.com create-job ArduinoBuildUsingMaven --username user --password pass < config.xml

If you don't get any errors you should see the job in your Jenkins configuration

![](https://dl.dropboxusercontent.com/u/13246619/ArduinoBuildJenkins/jenkins-job-list.png)

If you click on the job you should be able to see its details. As you can see this is a parameterized build, allowing you to specify a branch, a minorVersion and a MajorVersion.

![](https://dl.dropboxusercontent.com/u/13246619/ArduinoBuildJenkins/jenkins-job-detail.png)

### Add your Github url

As the template doesn't contain the actual github url of your project, you need to provide the correct location.

![](https://dl.dropboxusercontent.com/u/13246619/ArduinoBuildJenkins/empty_git.png)

### Launching the job.

When launching the job, you'll be able to provide

- The branch name where the build should take place
- The majorVersion we should assign to our sketch
- The minorVersion we should assign to our sketch

![](https://dl.dropboxusercontent.com/u/13246619/ArduinoBuildJenkins/jenkins-job-launch.png)

If everything goes according to plan your build should work fine:

![](https://dl.dropboxusercontent.com/u/13246619/ArduinoBuildJenkins/jenkins-job-ok.png)



## References

- http://jenkins-ci.org/
- http://www.arduino.cc/
- http://ant.apache.org/

