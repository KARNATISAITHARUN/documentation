# Authenticating with ShiftLeft

To use ShiftLeft Inspect and ShiftLeft Protect, you must first authenticate with ShiftLeft. There are three methods for authenticating with ShiftLeft, by using:

* [The Welcome page](#using-the-welcome-page)
* [The ShiftLeft CLI](#using-the-cli-to-authenticate) 
* [Environment Variables](#using-environment-variables-to-authenticate)

Once you have logged into ShiftLeft, you can obtain your credentials on the **Profile** page of the ShiftLeft Dashboard.

## Using the Welcome Page

You can use the Welcome page to authenticate only if you are running ShiftLeft on either the Linux or MacOS X operating systems.The [Welcome page](https://www.shiftleft.io/dashboard) is displayed the first time you log into ShiftLeft.

1. Copy the command.

   ![Location of Code to Authenticate](img/authenticate.jpg)

2. Run the command. The command automatically includes your unique organization ID and access token.

## Using the CLI to Authenticate

The CLI command `sl auth` is used to authenticate with ShiftLeft and associate your applications with your organization.

When you run `sl auth` without any arguments you are prompted for the credentials:

* ShiftLeft **Organization ID**
* ShiftLeft **Access Token**

Alternatively you can use `sl auth --org "$ORG" --token "$TOKEN"` with the same values.

Once authenticated, the CLI creates the local file `$HOME/.shiftleft/config.json` for Linux and MacOS X and `%HOME%/.shiftleft/config.json` for Windows. This file contains your Organization ID and Access Token, and is required by the CLI to use ShiftLeft Inspect and run the `sl analyze` command. For Linux and MacOS X, if the `$HOME` environment variable is not set locally, use the path `/etc/shiftleft/config.json`.

## Using Environment Variables to Authenticate

If you are using a CI/CD tool to submit applications to ShiftLeft Inspect for analysis, create the following environment variables to automate the authentication process: 

Environment variable for **Organization ID**:
- Name: `SHIFTLEFT_ORG_ID`
- Value: `{org-id-string}`

Environment variable for **Access Token**:
- Name: `SHIFTLEFT_ACCESS_TOKEN`
- Value: `{access-token-string}`

Note: Upload Token was replaced by Access Token in `sl --version` v0.7.1030. For `sl --version` prior to v.0.7.1030 please upgrade or use:

Environment variable for **Upload Token**:
- Name: `SHIFTLEFT_UPLOAD_TOKEN`
- Value: `{upload-token-string}`
