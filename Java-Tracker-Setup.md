### Downloads

There are a few ways to get the Java tracker started. Adding the jar file to your project dependencies is the recommended method of getting started. Since production is still early and large changes are being made daily, jar files may not be up to date. Check the version associated with the jar. Class files can be found in the out folder. At the moment the source files are set up as a package.

#### Easiest Method - Jar:

The quickest and easiest way to incorporate the Snowplow Java Tracker into your project is downloading all the files using

    git clone https://github.com/snowplow/snowplow.git
    cd snowplow/1-trackers/java-tracker
## Current Build

Since production is still early and large changes are being made daily, jar files may not be up to date, check the version associated with the jar. Class files can be found in the out folder. At the moment the source files are set up as a package.

To get started, move into the src folder and follow the set up guide!

Note: The set up there is for the package "com.snowplow.javaplow", this means you should leave all source files in the folders, and drag the entire package folder into your project. 

## Quick Start

There are a few ways to get the Java tracker started. Since production is still early and large changes are being made daily, I have not uploaded class or jar files yet. At the moment they files are set up as a package.

To get started, download the javaplow package and install the three library dependencies [Here] [dependencies].

Place the javaplow package in your projects src folder or root. If performed correctly you should be ready to instantiate a tracker.

More documentation on functions will come soon, but to instantiate a tracker in your code (can be global or local to the process being tracked) simply put the following:



You should now see all the java tracker files, including a folder called JavaPlow_VERSION_jar. Add the jar file in there to your projects dependencies.

Here are setup options for popular IDEs: [IntelliJ 13][intellij] or [Eclipse][eclipse].

Thats it! Skip down to setting up the tracker.

#### Source Files: 

To get started with the javaplow source files, download the javaplow package and either add the javaplow jar file to the list of dependencies or install the three library dependencies [Here][dependencies].

Place the javaplow package in your projects src folder or root. If performed correctly you should be able to compile and ready to instantiate a tracker.

