![SAP UI5 YouTube Course/ Tutorial](https://user-images.githubusercontent.com/19891236/95460237-996b5c00-096c-11eb-9417-b15a384e098c.png)

# SAP UI5 walkthrough

This is an example app that has been set up for automated unit and integration testing via Karma. 
The intetino is for it to be used in an Azure Devops pipelines  
 
# If you want to add Karma to your SAPUI5, this is the quick recipe

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
## commands
 - npm install --global karma-cli
 - npm i
 - npx browserslist@latest --update-db
 - karma start


## UI5-App including config for Karma testrunner 

This repository contains a simple UI5 application with few trivial QUnit tests defined. Further, there is a Karma config included which executes the UI5 tests and is also executable from a CI-Pipeline like Azure-Pipelines. The configuration is tested with Azure-Pipelines and the repository includes also the Azure-Pipeline configuration.

### Install required modules
In order to use Karma, you need to install required npm modules. Run the install from the root folder of the repository.

```
    npm install
```

### Starting the generated app

-   This app has been generated using the SAP Fiori tools - App Generator, as part of the SAP Fiori tools suite.  In order to launch the generated app, simply run the following from the generated app root folder:

```
    npm start
```

### Start the Karma testrunner

```
    npm test
```
or
```
    karma start
```

### Include Karma testrunner in Azure Pipelines

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

### Pre-requisites:

1. Active NodeJS LTS (Long Term Support) version and associated supported NPM version.  (See https://nodejs.org)

### Reference
[Blog post on SCN](https://blogs.sap.com/2021/10/04/run-ui5-tests-with-karma-in-azure-pipelines/)
