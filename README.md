## Update creator tool for APIM 2.1.0

A bash script which communicates with GitHub, Jira and Jenkins REST APIs and generates file structure of a WUM update automatically for `jag`, `js`, `json`, `jar` and `war` files. This will download modified `jar` or REST API `war` files from jenkins and copies to correct location in the WUM update. This will also autogenerate `update-descriptor.yaml` for the modified files as above and include issues with summary information.

### Prerequisites

* Install jq.
```
$ sudo apt-get install jq
```

* Set a system variable SUPPORT_CARBON_APIMGT_HOME with the location to carbon-apimgt repo.
```
$ export SUPPORT_CARBON_APIMGT_HOME="/home/malintha/wso2apim/gitworkspace/supportgit/apim210/carbon-apimgt-1"
```

### Usage:

```sh
$ create-apim-update <github-token> <pull-requests-filename> <issues-filename> <update-number> <wso2-username>

# sample usage:
$ create-apim-update 5abd6ab787eb1d6f7723456da35cba235ba1234b pull-requests.txt issues.txt 1610 user@wso2.com
```

### Inputs:

* A github token. Refer https://github.com/settings/tokens
* A file containing a list of PRs (**pull-requests.txt**)
```
https://github.com/wso2-support/carbon-apimgt/pull/276
https://github.com/wso2-support/carbon-apimgt/pull/293
https://github.com/wso2-support/carbon-apimgt/pull/296
```
* A file containing a list of issue URLs (**issues.txt**)
```
https://wso2.org/jira/browse/APIMANAGER-5827
https://wso2.org/jira/browse/APIMANAGER-5872
https://github.com/wso2/product-apim/issues/1563
```

* Update number (**1610**)
* WSO2 email username (**user@wso2.com**)



### Outputs of the command:

* List of changed `jag`, `js` and `json` files are fetched from PRs and automatically copied to correct location.
* Updated `jar`,`war` files are downloaded from jenkins and automatically copied to `plugins` or `webapps` folder.
* `update-descriptor.yaml` is created with modified files and issue descriptions.

#### WUM Update folder generated for the above **pull-requests.txt** and **issues.txt**

```
WSO2-CARBON-UPDATE-4.4.0-9999
======================
WSO2-CARBON-UPDATE-4.4.0-9999
├── carbon.home
│   └── repository
│       └── deployment
│           └── server
│               └── jaggeryapps
│                   └── publisher
│                       └── site
│                           ├── conf
│                           │   └── locales
│                           │       └── jaggery
│                           │           └── locale_default.json
│                           └── themes
│                               └── wso2
│                                   └── templates
│                                       ├── item-implement
│                                       │   ├── initializer.jag
│                                       │   ├── js
│                                       │   │   └── api-implementation.js
│                                       │   └── template.jag
│                                       └── usage
│                                           └── template.jag
└── update-descriptor.yaml

16 directories, 6 files
```

#### `update-descriptor.yaml` generated for the above **pull-requests.txt** and **issues.txt**
```
update_number: 9999
platform_version: 4.4.0
platform_name: wilkes
applies_to: APIM 2.1.0
bug_fixes:
  APIMANAGER-5827: Description mismatch in API Publisher UI
  APIMANAGER-5872: Update the API using PUT method thumbnailUri get set to null, api summary not contains thumbnailUrl and resource are resetting after update only the thumbnail
  https://github.com/wso2/product-apim/issues/1563: Mediation sequence upload fails after updating tittle attribute of swagger definition
description: |
  This is a sample description. Please update.
file_changes:
  added_files: []
  removed_files: []
  modified_files:
  - repository/deployment/server/jaggeryapps/publisher/site/conf/locales/jaggery/locale_default.json
  - repository/deployment/server/jaggeryapps/publisher/site/themes/wso2/templates/item-implement/initializer.jag
  - repository/deployment/server/jaggeryapps/publisher/site/themes/wso2/templates/item-implement/js/api-implementation.js
  - repository/deployment/server/jaggeryapps/publisher/site/themes/wso2/templates/item-implement/template.jag
  - repository/deployment/server/jaggeryapps/publisher/site/themes/wso2/templates/usage/template.jag
```
