## ANT file for Arduino

This is a simple ANT file that can be used to build Arduino sketches.

## Preparing your source code

The build currently expects that the ino file contains the following imports and methods

	#include "Arduino.h"
	#include "configuration.h"
	void setup();
	void loop(); 

These need to be present at the top of the file.

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

### Executing the command

	java -jar hudson-cli.jar -s http://jenkins.mycompany.com create-job ArduinoBuildUsingMaven --username user --password pass < config.xml

If you don't get any errors you should see the job in your Jenkins configuration

![](https://dl.dropboxusercontent.com/u/13246619/ArduinoBuildJenkins/jenkins-job-list.png)

If you click on the job you should be able to see its details. As you can see this is a parameterized build, allowing you to specify a branch, a minorVersion and a MajorVersion.

![](https://dl.dropboxusercontent.com/u/13246619/ArduinoBuildJenkins/jenkins-job-detail.png)

The only thing you need to fill out is your git repository location,

![](https://dl.dropboxusercontent.com/u/13246619/ArduinoBuildJenkins/empty_gif.png)

## References

- http://jenkins-ci.org/
- http://www.arduino.cc/
- http://ant.apache.org/

