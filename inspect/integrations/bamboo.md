# How to Integrate Inspect with Bamboo

Bamboo is a continuous integration and deployment tool that ties automated builds, tests, and releases into a single workflow. You can integrate Inspect into your Bamboo workflow to provide automated code analysis.

## Prerequisites

This tutorial assumes that you have:

* [Installed Bamboo](https://confluence.atlassian.com/bamboo/bamboo-installation-guide-289276785.html)
* [Installed Inspect](/inspect/getting-started/README.md)

## Configuring Bamboo

Log into your Bamboo server using an account that has administrator privileges.

Create the following environment variables

| **Environment Variable** | **Value** |
| - | - |
| `SHIFTLEFT_ORG_ID` | Your Organization ID |
| `SHIFTLEFT_ACCESS_TOKEN` | Your Access Token |

You can find your Organization ID and Access Token in the ShiftLeft Dashboard under [Account Settings](https://www.shiftleft.io/user/profile).

Provide Bamboo instructions on running Inspect by [creating a shell script](https://confluence.atlassian.com/bamboo/script-289277046.html) a shell script that contains task running information. 

You will be asked to provide values for the following parameters when creating the script:

| **Parameter** | **Value** |
| - | - |
| Task description | `SL Analyze` (or similar) |
| Script body | `/usr/local/bin/sl analyze` or `/usr/local/bin/sl analyze - -cpg` |
| Working sub directory | `<path to where your build project packages>` |

Make sure that the script called to run Inspect is the last build task listed, since it should run after all of your other Bamboo tasks.
