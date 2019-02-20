# Config environments in Angular app to run builds on Azure DevOps (CI)

## angular.json
Add environments to angular.json config file:
```
{
  "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
  ...
      "configurations": {
      "production": {
        "fileReplacements": [
          {
            "replace": "src/environments/environment.ts",
            "with": "src/environments/environment.prod.ts"
          }
        ],
      ...
      },
      "staging": {
        "fileReplacements": [
          {
            "replace": "src/environments/environment.ts",
            "with": "src/environments/environment.staging.ts"
          }
        ],
  ...
}
```

## package.json
Config npm run buil commands from packages config:
```
{
  "name": "nome-app",
  "version": "1.0.0",
  "scripts": {
    "ng": "ng",
    "start": "ng serve",
    "build": "ng build",
    "test": "ng test",
    "lint": "ng lint",
    "e2e": "ng e2e",
    "build-staging": "ng build --configuration=staging --build-optimizer",
    "build-prod": "ng build --configuration=production --build-optimizer"
  },
...
```

## azure-pipelines.yml
At NPM Build tast, add commands configured in packages.json file:
```
...
- task: Npm@1
  displayName: Build
  inputs:
    workingDir: '$(AngularRootFolder)'
    command: custom
    verbose: false
    customCommand: 'run build-staging'
...
- task: Npm@1
  displayName: Build
  inputs:
    workingDir: '$(AngularRootFolder)'
    command: custom
    verbose: false
    customCommand: 'run build-prod'
...
```