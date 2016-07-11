<!--
/*  
 * Copyright IBM Corp. 2016
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 * http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */
-->
# Globalization Pipeline BETA Migration Guide

## Overview

The following information outlines how to migrate the translation stored
in an instance of the IBM Globalization Pipeline Beta service to a new
instance of the IBM Globalization Pipeline GA service.

## Step 1 - Download Globalization Pipeline CLI tool

This migration instruction utilizes [Globalization Pipeline
CLI tool](https://github.com/IBM-Bluemix/gp-java-tools). The command
used for migrating data is available gp-cli-1.1.0. To run the
tool, you need Java SE Runtime Environment 7 or later version.

## Step 2 - Save the BETA instance's credentials

Logon to Bluemix console and browse the service credentials used by
the BETA service instance. For more details, see [Credentials](README.md#4-credentials)
in Quick Start Guide.

Once you get the credential information, create a new json file
and store the credentials as below.
```
{
    "url":"https://gp-beta-rest.ng.bluemix.net/translate/rest",
    "userId":"50b84f636581b91ca0f96df896fc813d",
    "password":"iN6Pu2ywoaVEB7O84kvnEND/t6MTeyTy",
    "instanceId":"4e10de1652319ab6ba69f197d3c3ac76"
}
```
Note: Credentials above are example, and actual property values
are different.

In this instruction, we use *betacreds.json* as the file name.

## Step 3 - Creating a new Globalization Pipeline service

You need a new Globalization Pipeline service instance to host translation
bundles. To create a new service instance, please follow the steps
explained in [Quick Start Guide](README.md#quick-start-guide).

If the BETA instance was bound to your Bluemix Apps, you should also
bind the new instance.

After the new instance is created, browse the service credentials
as you did in the previous step for beta. You need to save the value
of *url*, *userId*, *password* and *instanceId*. (Note: The command used
for transferring data to a new instance does not take target service
instance's credentials in JSON file. So you don't need to create a
JSON file for the new service instance.)

## Step 4 - Run copy-all-bundles command

Run copy-all-bundles command as below.
```
java -jar cp-cli-1.1.0.jar copy-all-bundles --dest-url https://gp-rest.ng.bluemix.net/translate/rest --dest-instance-id 9146abf71bb94513504a0eaf76d57804 --dest-user-id 52858e19ae57ba6f2d2ea7e38e9ab457 --dest-password o75YXQCK2obQLOvedkSslBTAyeUq7/+t -j betacreds.json
```
Above is an example. Parameter values should be replaced with
actual credentials you saved in Step 2 and 3.

* --dest-url - *url* in the instance's credentials (Step 3)
* --dest-instance-id - *instanceId* in the instance's credentials (Step 3)
* --dest-user-id - *userId* in the instance's credentials (Step 3)
* --dest-password - *password* in the instance's credentials (Step 3)
* --j - BETA instance's credentials stored in a JSON file (Step 2)

Below is an example of command output.
```
.. copying bundle: MyMessages1
.. copying bundle: MyMessages2
.. copying bundle: MyMessages3
All bundles were successfully copied to the specified service instance.
```

## Step 5 - Update credentials in your application code (optional)

If your application dynamically access the BETA instance using one of
Globalization Pipeline SDK previously, and service credentials are
manually set in your application, replace the one for BETA with the
new instance's credentials.

Also, you may want to update the client SDK to the latest version,
although previous version client SDK released for BETA service
should continue to work.
