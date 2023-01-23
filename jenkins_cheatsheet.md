# Jenkins
Jenkins uses Groovy to run pipeline jobs and inherits some properties of Groovy, including _string interpolation_

## __String interpolation__
- Use __double quotes__ (") for expansion to inject in variable values
- use __single quotes__ (') for string literals
- use __triple quotes__ (''') for multiline shell scripts


## Master
Performs the admin work in a job, firing job triggers, and storing job data

## Agent
Performs the build process outlined in a job

# Jenkinsfile
Configuration file outlining a __Jenkins Job__, the build process for an application. This file should live in your source code repository

__Jenkinsfile Content__

- Triggers (when to run the build steps)
- Build environment (certificates and credentials)
- Build (testing and packaging)
- Post-build (Publishing packages and deployment


## Blocks
Denote the different sections of the jenkinsfile (pipeline, stages, steps, etc...)


## Pipeline
The first block of the jenkinsfile. There should be a one-to-one mapping, _one pipeline per jenkinsfile_

- _agent_ - the agent that will run the job
- _environment variables_ - declare environment variables that can be used throughout the jenkins job


## Stages
The next blocks under the pipeline block where the structure of the pipeline is defined. The different stages can include test, build, deploy, etc.

- _name_ - name of the current stage block
- _agent_ - the agent to run the specific stage
- _environment variables_ - the env variables for use in the stage


## Steps
The functions that will be performed in the stage. 

- commands
- shell scripts (sh)


## Jenkinsfile Structure
___NOTE:___ Jenkinsfile supports _string interpolation_. Use __double quotes__ (") for expansion, use __single quotes__ (') for string literals, and use __triple quotes__ (''') for multiline shell scripts

```bash
stage('test') {
	environment {
		my_var='expansion'
	}
	steps {
		// expansion
		echo "jenkinsfile uses ${my_var}"
		// outputs - jenkinsfile uses expansion

		// no expansion - string literal
		echo 'jenkinsfile uses ${my_var}'
		// outputs - jenkinsfile uses ${my_var}

		// multi-line sccript
		sh '''
			echo "Running tests"
			cd ./tests
			pytest -m -v
		'''
	}
```


Pipeline
    |____ Stage 1
    |____ Stage 2
	    |____ Step 2a
	    |____ Step 2b

```bash
pipeline {
	agent any
	environment {
		MY_VAR="some $value"
	}

	stages {
		stage('stage-1') {
			steps {
				echo "step 1 of stage-1 $MY_VAR"
			}
		}
		stage{'test') {
			steps {
				echo 'running my tests'
			}
		}
	}

```


# Shared Libraries
Place to hold Groovy scripts that can be accessed by multiple Jenkinsfiles. Move Groovy scripts from Jenkinsfiles into their own script files (.groovy files)

## Shared Library Structure
- `/src` - contains Groovy source files
- `/resources` - static data and config files
- `/vars` - _global_ Groovy scripts

## Groovy Scripts for Jenkins Shared Library
- the `shared library` containing the scripts __MUST__ be in a separate source code repository, not in our project with the Jenkinsfile
- __ALL__ groovy scripts must be put inside a folder named `vars` in our shared library repository
- methods have to be named `call` (default name Jenkins will use when invoking the custom methods)
- code inside a call method has to be wrapped in a `node` block (makes it usable as a pipeline step)
- name of script file needs to be the name we use in the jenkinsfile step

```groovy
// shared_library/vars/auditTools.groovy - file path location must be in a 'vars' directory

// auditTools.groovy - file name to be used in jenkinsfile step

// method must be named 'call'
def call() {
	// code must be wrapped in 'node' block
	node {
		sh '''
		  git version
		  docker version
		  dotnet --list-sdks
		  dotnet --list-runtimes
	}
}


// scripts can also contain an entire pipeline

// simplePipeline.groovy

def call() {
	pipeline {
		agent any
		environment {
			MODULE='m4'
		}
		stages {
			stage('Verify') {
				steps {
					echo "Module: ${MODULE}"
					sh 'git version'
				}
			}
		}
	}
}

```

# Jenkinsfile Linter API
Comes bundled with the pipeline plugin. Send a jenkinsfile via a POST request and receive a validation message

## __POST request format__
`curl -X POST -F "jenkinsfile=<./Jenkinsfile" <jenkins-server-name>/pipeline-model-converter/validate`

```bash
# bash

# example where the jenkins server were running on our local machine on localhost port 8080
curl -X POST -F "jenkinsfile=<./Jenkinsfile" http://localhost:8080/pipeline-model-converter/validate
```

_Note:_ The Jenkins server is the url where your Jenkins Jobs are located
