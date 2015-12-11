SCUFL2 examples
===============

Examples of using the [SCUFL2 API](http://dev.mygrid.org.uk/wiki/display/developer/SCUFL2+API) v0.14.0.

[![Build Status](https://travis-ci.org/myGrid/scufl2-examples.svg?branch=master)](https://travis-ci.org/myGrid/scufl2-examples)

As this code is meant only as examples that you can extend and modify, the
groupId and package name are `org.apache.taverna.examples`, which should be changed
in your application.



Download
--------
Download the `scufl2-examples-0.x.x-standalone.jar` from the
[latest build](http://build.mygrid.org.uk/ci/job/scufl2-examples/lastSuccessfulBuild/com.example$scufl2-examples/), or browse the [Maven snapshot repository](http://repository.mygrid.org.uk/artifactory/libs-snapshot-local/com/example/scufl2-examples/).

This is a standalone executable JAR (see below). 



Build
-----


To build, you'll need [Maven](http://maven.apache.org/download.cgi) 3.0.5 or newer, and run
```mvn clean install```:

    C:\Users\stain\workspace\scufl2-examples> mvn clean install
    
    [INFO] ------------------------------------------------------------------------
    [INFO] Building SCUFL2 examples 0.1.1-SNAPSHOT
    [INFO] ------------------------------------------------------------------------
    [INFO] 
    [INFO] --- maven-clean-plugin:2.4.1:clean (default-clean) @ scufl2-examples ---
    [INFO] Deleting C:\Users\stain\workspace\scufl2-examples\target
    (..)
    [INFO] Installing C:\Users\stain\workspace\scufl2-examples\target\scufl2-examples-0.1.1-SNAPSHOT.jar to C:\Users\stain\.m2\repository\com\example\scufl2-examples\0.1.1-SNAPSHOT\scufl2-examples-0.1.1-SNAPSHOT.jar
    [INFO] Installing C:\Users\stain\workspace\scufl2-examples\pom.xml to C:\Users\stain\.m2\repository\com\example\scufl2-examples\0.1.1-SNAPSHOT\scufl2-examples-0.1.1-SNAPSHOT.pom
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 6.223s
    [INFO] Finished at: Tue Apr 23 11:00:27 BST 2013
    [INFO] Final Memory: 15M/243M
    [INFO] ----------------    

To run the examples, see the folder ```target\scufl2-examples``` which has a `bin/` executable for each of the below tools. Note that these tools require the provided `bin/` and `lib/` folder structure.

Alternatively you can use ```java -jar target/scufl2-examples-0.2.0-SNAPSHOT-standalone.jar``` - this is a standalone
JAR file that can execute any of the tools as the first argument. 

Linux users can install this with:

    chmod 755 target/scufl2-examples-*-standalone.jar
    sudo cp target/scufl2-examples-*-standalone.jar /usr/local/bin/scufl2tool
    
as long as JAR files are recognized as executables:

    stain@biggie-mint ~/src/scufl2-examples $ target/scufl2-examples-0.2.0-SNAPSHOT-standalone.jar 
    SCUFL2 workflow tool
    Usage: scufl2tool <tool> [option] ...

    Available tools:
    t2flowtowfbundle - Convert t2flow workflows to wfbundle
    jsonexport - Export JSON structure of workflow
    processornames - List tree of processor names in workflow
    servicetypes - List service types used in workflow
    workflowmaker - Create an example workflow programmatically



processornames
--------------
This tool lists a tree of the processor names in the workflows of the given bundle.

    C:\Users\stain\workspace\scufl2-examples> target\scufl2-examples\bin\processornames examples/helloworld.t2flow
    + Hello_World
      - hello
    
    
    C:\Users\stain\workspace\scufl2-examples> target\scufl2-examples\bin\processornames examples/helloanyone.t2flow
    + Hello_Anyone
      - Concatenate_two_strings
      - hello

See the source code for [org.apache.taverna.examples.ProcessorNames](src/main/java/org/apache/taverna/examples/ProcessorNames.java)
 for how this is implemented.


servicetypes
------------
This tool lists the URIs of the types of services (activities) used in the workflow.

    C:\Users\stain\workspace\scufl2-examples> target\scufl2-examples\bin\servicetypes examples/helloanyone.t2flow
    http://ns.taverna.org.uk/2010/activity/beanshell
    http://ns.taverna.org.uk/2010/activity/constant

See the source code for [org.apache.taverna.examples.ServiceTypes](src/main/java/org/apache/taverna/examples/ServiceTypes.java)
 for how this is implemented.


workflowmaker
-------------
This tool shows how to construct a workflow from scratch and save it as a 
[SCUFL2 wfbundle](http://dev.mygrid.org.uk/wiki/display/developer/Taverna+Workflow+Bundle).

    C:\Users\stain\workspace\scufl2-examples> target\scufl2-examples\bin\workflowmaker
    Written to C:\Users\stain\AppData\Local\Temp\test6264603033381329995.wfbundle
    
    WorkflowBundle '4fdb73e3-a5c6-41a8-aafb-32f67ca552b2'
      MainWorkflow 'Echotest'
      Workflow 'Echotest'
        In 'in1'
        Out 'out1'
        Processor 'p'
          In 'pIn'
          Out 'pOut'
        Links
          'in1' -> 'p:pIn'
          'p:pOut' -> 'out1'
      MainProfile 'default'
      Profile 'default'
        Activity 'myBeanshell'
          Type <http://ns.taverna.org.uk/2010/activity/beanshell>
          In 'in1'
          Out 'out1'
        ProcessorBinding '434e7226-1d7f-498d-a19f-75be9f38f266'
          Activity 'myBeanshell'
          Processor 'Echotest:p'
          InputPortBindings
            'pIn' -> 'in1'
          OutputPortBindings
            'out1' -> 'pOut'
        Configuration 'beanshellConf'
          Type <http://ns.taverna.org.uk/2010/activity/beanshell#Config>
          Configures 'activity/myBeanshell'
          Property <http://ns.taverna.org.uk/2010/activity/beanshell#script>
            '''out1 = in1'''


See the source code for [org.apache.taverna.examples.WorkflowMaker](src/main/java/org/apache/taverna/WorkflowMaker.java) 
for how this is implemented.


t2flowtowfbundle
----------------

This tool shows how to convert a *.t2flow* file to a 
[SCUFL2 .wfbundle](http://dev.mygrid.org.uk/wiki/display/developer/Taverna+Workflow+Bundle).

    C:\Users\stain\workspace\scufl2-examples> rm helloworld.wfbundle
    
    C:\Users\stain\workspace\scufl2-examples> target\scufl2-examples\bin\t2flowtowfbu
    
    C:\Users\stain\workspace\scufl2-examples> unzip -t helloworld.wfbundle
    Archive:  helloworld.wfbundle
        testing: mimetype                 OK
        testing: META-INF/                OK
        testing: META-INF/manifest.xml    OK
        testing: history/                 OK
        testing: history/8781d5f4-d0ba-48a8-a1d1-14281bd8a917.t2flow   OK
        testing: workflow/                OK
        testing: workflow/Hello_World.rdf   OK
        testing: profile/                 OK
        testing: profile/taverna-2.2.0.rdf   OK
        testing: annotation/              OK
        testing: annotation/0c2f3c2c-7b5f-49cc-a448-92d707da3795.rdf   OK
        testing: annotation/48c04fe7-90f8-42f2-be61-a0480aa826d1.rdf   OK
        testing: annotation/9558f2e4-b629-4d1d-a62e-8c0dd39d7746.rdf   OK
        testing: workflowBundle.rdf       OK
        testing: META-INF/container.xml   OK
    No errors detected in compressed data of helloworld.wfbundle.

See the source code for 
[org.apache.taverna.examples.ConvertT2flowToWorkflowBundle.java](src/main/java/org/apache/taverna/examples/ConvertT2flowToWorkflowBundle.java)
for how this is implemented.

The included files [helloworld.wfbundle](helloworld.wfbundle?raw=true) and
[helloanyone.wfbundle](helloanyone.wfbundle?raw=true) are examples of converting
[helloworld.t2flow](helloworld.t2flow?raw=true) and [helloanyone.t2flow](helloanyone.t2flow?raw=true)  


jsonexport
----------
This tool exports a JSON structure of the workflow, including basic
annotations on workflows, ports, processors, nested workflows and
revision log of the workflows.

Usage:

    c:\Users\stain\src\scufl2-examples> target\scufl2-examples\bin\jsonexport -h
    Export workflow structore as JSON.
    Usage: jsonexport [filename] ...
    If the filename is - the workflow will be read from STDIN and
    JSON written to STDOUT.
    Otherwise, the file is read as a workflow (t2flow, workflow bundle)
    and written as JSON to a file with the .json extension.
    Multiple filenames can be given. JSON filenames are written to STDOUT

Converting multiple files:

    c:\Users\stain\src\scufl2-examples> target\scufl2-examples\bin\jsonexport examples/helloworld.t2flow examples/helloanyone.wfbundle
    examples/helloworld.json
    examples/helloanyone.json
    
See the converted [examples/helloworld.json](helloworld.json) and
[examples/helloanyone.json](helloanyone.json).

Example using STDIN/STDOUT:

    c:\Users\stain\src\scufl2-examples> target\scufl2-examples\bin\jsonexport - < examples/helloworld.t2flow
    {
      "@context" : [ "https://w3id.org/scufl2/context", {
        "@base" : "http://ns.taverna.org.uk/2010/workflowBundle/8781d5f4-d0ba-48a8-a1d1-14281bd8a917/"
      } ],
      "id" : "http://ns.taverna.org.uk/2010/workflowBundle/8781d5f4-d0ba-48a8-a1d1-14281bd8a917/",
      "workflow" : {
        "id" : "workflow/Hello_World/",
        "name" : "Hello_World",
        "revisions" : [ {
          "id" : "http://ns.taverna.org.uk/2010/workflow/8781d5f4-d0ba-48a8-a1d1-14281bd8a917/",
          "generatedAtTime" : "2012-01-03T15:12:21Z"
        } ],
        "inputs" : [ ],
        "outputs" : [ {
          "name" : "greeting",
          "id" : "workflow/Hello_World/out/greeting"
        } ],
        "processors" : [ {
          "id" : "workflow/Hello_World/processor/hello/",
          "name" : "hello",
          "inputs" : [ ],
          "outputs" : [ {
            "name" : "value",
            "id" : "workflow/Hello_World/processor/hello/out/value",
            "depth" : 0
          } ]
        } ],
        "datalinks" : [ {
          "receivesFrom" : "workflow/Hello_World/processor/hello/out/value",
          "sendsTo" : "workflow/Hello_World/out/greeting"
        } ],
        "controllinks" : [ ],
        "http://purl.org/dc/terms/description" : "One of the simplest workflows possible. No workflow input ports, a single workflow output port \"greeting\",  outputting \"Hello, world!\" as produced by the String Constant \"hello\".",
        "http://purl.org/dc/elements/1.1/creator" : "Stian Soiland-Reyes",
        "http://purl.org/dc/terms/title" : "Hello World"
      }
    }

