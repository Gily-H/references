# Jenkins
Jenkins uses Groovy to run pipeline jobs and inherits some properties of Groovy, including _string interpolation_

### __String interpolation__
- Use __double quotes__ (") for expansion to inject in variable values
- use __single quotes__ (') for string literals
- use __triple quotes__ (''') for multiline shell scripts

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
