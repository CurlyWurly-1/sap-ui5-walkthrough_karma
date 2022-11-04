# SAP UI5 walkthrough

This is an example app that has been set up for automated unit and integration testing via Karma. 
The purpose of this repo is to act as a guide on how to set up a UI5 app for automated UI5 testing within an Azure Devops pipeline  
It is a combination of [Brandon's example app](https://youtu.be/mmSB85rWQ3w) with [Michael's blog on how to add Karma](https://blogs.sap.com/2021/10/04/run-ui5-tests-with-karma-in-azure-pipelines/).

This repository contains a simple UI5 application with few trivial QUnit tests. Further, there is a Karma config included which executes the UI5 tests which are also executable from a CI-Pipeline like Azure-Pipelines. The configuration is tested with Azure-Pipelines.
N.B. The repository also includes the Azure-Pipeline configuration.

## Install commands
 - npm install --global karma-cli
 - npm install
 - npm i
 - npx browserslist@latest --update-db
 - karma start

## Starting the app
 - npm start

## Starting the Karma testrunner
 - npm test

or

 - karma start

## Include Karma testrunner in Azure Pipelines

```
    - task: PublishTestResults@2
        inputs:
            testResultsFormat: 'JUnit'
            testResultsFiles: '**/TESTS-*.xml'
        displayName: 'Display Unit Test result'

        - task: PublishCodeCoverageResults@1
        inputs:
            codeCoverageTool: 'Cobertura'
            summaryFileLocation: '$(Build.SourcesDirectory)/coverage/**/cobertura.xml'
```


# If you just want to add automated Karma to an existing SAPUI5 app, this is the quick recipe of changes you need to merge into your app

## Amendments
 - Add ./karma.conf.js
 - Add ./test/testsuite.qunit.html
 - Add ./test/testsuite.qunit.js
 - Add the following to "package .json"
```
	"scripts": {
		"start": "fiori run --open 'index.html'",
		"start-local": "fiori run --config ./ui5-local.yaml --open 'index.html'",
		"start-noflp": "fiori run --open 'index.html'",
		"build": "ui5 build -a --clean-dest --include-task=generateManifestBundle generateCachebusterInfo",
		"deploy": "fiori verify",
		"deploy-config": "fiori add deploy-config",
		"test": "karma start",
		"unit-tests": "fiori run --open test/unit/unitTests.qunit.html",
		"int-tests": "fiori run --open test/integration/opaTests.qunit.html"
	},
	"devDependencies": {
		"@sap/ux-ui5-tooling": "1",
		"@ui5/cli": "^2.11.1",
		"@ui5/fs": "^2.0.6",
		"@ui5/logger": "^2.0.1",
		"karma": "^6.3.4",
		"karma-chrome-launcher": "^3.1.1",
		"karma-coverage": "^2.0.3",
		"karma-junit-reporter": "^2.0.1",
		"karma-ui5": "^2.3.4",
		"puppeteer": "^10.2.0",
		"rimraf": "3.0.2"
	},
```
