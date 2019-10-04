# Authenticating with ShiftLeft

To use ShiftLeft Inspect and ShiftLeft Protect, you must first authenticate with ShiftLeft. There are two methods for authenticating, by using:

* [The ShiftLeft CLI](#using-the-shiftleft-cli-to-authenticate) 
* [Environment Variables](#using-environment-variables-to-authenticate)

The first time you log into ShiftLeft, if you are running ShiftLeft on either Linux or MacOS X, you obtain your authentication credentials (organization ID and access token) from the **Welcome** page of the ShiftLeft Dashboard. 

   ![Location of Code to Authenticate](img/authenticate.jpg)

Subsequently, you can obtain your credentials from the **Profile** page of the ShiftLeft Dashboard.

Once authenticated, the ShiftLeft CLI creates the local file `$HOME/.shiftleft/config.json` for Linux and MacOS X and `%HOME%/.shiftleft/config.json` for Windows. This file contains your Organization ID and Access Token, and is required by the ShiftLeft CLI to use ShiftLeft Inspect and run the `sl analyze` command. For Linux and MacOS X, if the `$HOME` environment variable is not set locally, the CLI uses the path `/etc/shiftleft/config.json`.

## Using the ShiftLeft CLI to Authenticate

The ShiftLeft CLI command `sl auth` is used to authenticate with ShiftLeft and associate your applications with your organization.

When you run `sl auth` without any arguments you are prompted for the credentials:

* ShiftLeft **Organization ID**
* ShiftLeft **Access Token**

Alternatively you can use `sl auth --org "$ORG" --token "$TOKEN"` with the same values.

## Using Environment Variables to Authenticate

If you are using a CI/CD tool to submit applications to ShiftLeft Inspect for analysis, create the following environment variables to automate the authentication process: 

Environment variable for **Organization ID**:
- Name: `SHIFTLEFT_ORG_ID`
- Value: `{org-id-string}`

Environment variable for **Access Token**:
- Name: `SHIFTLEFT_ACCESS_TOKEN`
- Value: `{access-token-string}`

Note that the Upload Token was replaced by the Access Token in `sl --version` v0.7.1030. For `sl --version` prior to v.0.7.1030 please upgrade or use 

Environment variable for **Upload Token**:
- Name: `SHIFTLEFT_UPLOAD_TOKEN`
- Value: `{upload-token-string}`
