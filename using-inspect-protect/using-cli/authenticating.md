# Authenticating with ShiftLeft

To use ShiftLeft Inspect and ShiftLeft Protect, you must first authenticate with ShiftLeft. Generally, you authenticated as part of the Quick Start process the first time you logged into ShiftLeft. 

![Quick Start Authentication](../img/authenticate.jpg)

You can also authenticate by using either: 

* [The ShiftLeft CLI](#using-the-cli-to-authenticate) 
* [Environment Variables](#using-environment-variables-to-authenticate)

## Using the CLI to Authenticate

The CLI command `sl auth` is used to to authenticate with ShiftLeft and associate your applications with your organization.

When you run `sl auth` without any arguments you are prompted for the credientials:

* ShiftLeft **Organization ID**
* ShiftLeft **Access Token**

Alternatively using `sl auth --org "$ORG" --token "$TOKEN"` with the same values is possible too and for ease of use this is also what is provided in the ShiftLeft Dashboard on the Account page.

Once authenticated, the CLI creates the local file `$HOME/.shiftleft/config.json` that contains your Organization ID and Access Token. The CLI requires this file to use ShiftLeft Inspect and run the `sl analyze` command. If the `$HOME` environment variable is not set locally, use the following path: `/etc/shiftleft/config.json`.

## Using Environment Variables to Authenticate

If you are using a CI/CD tool to submit applications to ShiftLeft Inspect for analysis, create the following environment variables to automate the authentication process: 

Environment variable for **Organization ID**:
- Name: `SHIFTLEFT_ORG_ID`
- Value: `{org-id-string}`

Environment variable for **Access Token**:
- Name: `SHIFTLEFT_ACCESS_TOKEN`
- Value: `{access-token-string}`

*Note: Upload Token was replaced by Access Token in `sl --version` v0.7.1030 .*
*For `sl --version` prior to v.0.7.1030 please upgrade, or use:*

Environment variable for **Upload Token**:
- Name: `SHIFTLEFT_UPLOAD_TOKEN`
- Value: `{upload-token-string}`

## Obtaining Your Credentials

Once you have logged into ShiftLeft, you can obtain your credentials on the **Profiles** page of the ShiftLeft Dashboard.
