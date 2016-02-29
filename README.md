
# [![**Brooklyn**](https://brooklyn.apache.org/style/img/apache-brooklyn-logo-244px-wide.png)](http://brooklyn.apache.org/)

### Apache Brooklyn Client CLI

A command line client for [Apache Brooklyn](https://brooklyn.apache.org).

## Toolchain

You will need the following tools to build the CLI:
- Go (min version 1.5.1), with full cross-compiler support (see https://golang.org/dl).
  On Mac, if using Homebrew, use "brew install go --with-cc-all"
- godep (see https://github.com/tools/godep)

Optional:
- Maven (used by the Brooklyn build process)


## Build Pre-Requisites

- Ensure your [$GOPATH](http://golang.org/cmd/go/#hdr-GOPATH_environment_variable) is set correctly 
  to a suitable location for your Go code.
- Install Brooklyn CLI and dependencies (note the "-d" parameter, which instructs Go to download the files but not
  build the code).  
`go get -d github.com/apache/brooklyn-client/br`  
    
    
## A note on dependency management

The CLI has a small number of dependencies, notably on codegansta/cli.  To manage the version of dependencies, the CLI
code currently uses godep.  When contributing to the CLI it is important to be aware of the distinction.  To avoid
potentially bringing in new versions of dependencies, use "godep go" to build the code using the dependencies
saved in br/Godeps.  Alternatively, to bring in the latest versions of the dependencies, build with "go get", but in
that case remember to update the dependencies of the project using "godep save" along with your commit.

## Compiling the code with Go for development purposes


### Using saved dependencies
As Go dependendencies for godep are held in the main package directory ("br"), you need to build from that directory,
using godep:

```bash
cd $GOPATH/src/github.com/apache/brooklyn-client/br
godep go install 
```
This will build the "br" executable into $GOPATH/bin

### Updating the dependencies

To use the latest published versions of the dependencies simply use 
```bash
go get github.com/apache/brooklyn-client/br
```

When the code is ready to be committed, first update the saved dependencies with
```bash
cd $GOPATH/src/github.com/apache/brooklyn-client/br
godep save
```


## Building the code for release

Either:
- Use the build script in the "release" folder directly (see its usage for details), or
- Invoke the build script via Maven with one of 
  - ```mvn clean install```                                     build for all supported platforms
  - ```mvn -Dtarget=native clean install```                     build for the current platform
  - ```mvn -Dtarget=cross -Dos=OS -Darch=ARCH clean install```  build for platform with operating system OS and architecture ARCH

This builds the requested binaries into the "target" directory, each with a file name that includes the version,
timestamp, and architecture details, e.g. br.0.9.0.20151218-195906.linux.amd64, and installs a zip file containing them
all as a maven artifact.  To run any of these as "br" of course you will need to create an alias or soft link.

## Running

Ensure your path contains $GOPATH/bin.

First, log in to your Brooklyn instance with:

    $ br login URL [USER PASSWORD]

See the help command for info on all commands:

    $ br help

And for help on individual commands:

    $ br help COMMAND


## Scopes
   Many commands require a "scope" expression to indicate the target on which they operate. The scope expressions are
   as follows (values in brackets are aliases for the scope):
   - application APP-ID   (app, a)  
     Selects an application, e.g. "br app myapp"
   - entity      ENT-ID   (ent, e)  
     Selects an entity within an application scope, e.g. "br app myapp ent myserver"
   - effector    EFF-ID   (eff, f)  
     Selects an effector of an entity or application, e.g. "br a myapp e myserver eff xyz"
   - config      CONF-KEY (conf, con, c)  
     Selects a configuration key of an entity e.g. "br a myapp e myserver config jmx.agent.mode"
   - activity    ACT-ID   (act, v)  
     Selects an activity of an entity e.g. "br a myapp e myserver act iHG7sq1"


## Commands

   Commands whose description begins with a "*" character are particularly experimental and likely to change in upcoming
   releases.  If not otherwise specified, "SCOPE" below means application or entity scope.  If an entity scope is not
   specified, the application entity is used as a default.

   - *access*         Show access control
   - *activity*       Show the activity for an application / entity
   - *add-catalog*    * Add a new catalog item from the supplied YAML
   - *add-children*   * Add a child or children to this entity from the supplied YAML
   - *application*    Show the status and location of running applications
   - *catalog*        * List the available catalog applications
   - *config*         Show the config for an application or entity
   - *delete*         * Delete (expunge) a brooklyn application
   - *deploy*         Deploy a new brooklyn application from the supplied YAML
   - *destroy-policy* Destroy a policy
   - *effector*       Show the effectors for an application or entity
   - *entity*         Show the entities of an application or entity
   - *env*            Show the ENV stream for a given activity
   - *invoke*         Invoke an effector of an application and entity
   - *locations*      * List the available locations
   - *login*          Login to brooklyn
   - *policies*       Show the list of policies for an application and entity
   - *policy*         Show the status of a policy for an application and entity
   - *rename*         Rename an application or entity
   - *restart*        Invoke restart effector on an application and entity
   - *sensor*         Show values of all sensors or named sensor for an application or entity
   - *set*            Set config for an entity
   - *spec*           Get the YAML spec used to create the entity, if available
   - *start*          Invoke start effector on an application and entity
   - *start-policy*   Start or resume a policy
   - *stderr*         Show the STDERR stream for a given activity
   - *stdin*          Show the STDIN stream for a given activity
   - *stdout*         Show the STDOUT stream for a given activity
   - *stop*           Invoke stop effector on an application and entity
   - *stop-policy*    Suspends a policy
   - *tree*           * Show the tree of all applications
   - *version*        Display the version of the connected Brooklyn
   - *help*    


----
Licensed to the Apache Software Foundation (ASF) under one 
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

 http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY 
KIND, either express or implied.  See the License for the 
specific language governing permissions and limitations
under the License.
