# Getting Started with Inspect (Linux and macOS)

This tutorial shows you how to install, set up, and configure Inspect.

## Register for a ShiftLeft Account

If you haven't already, [register](https://shiftleft.io/register) for a ShiftLeft account.

You will be prompted to create an organization. Provide a name for your organization and click **Create Organization** to proceed.

## Install ShiftLeft

As a new ShiftLeft user, you will be presented with a list of steps that you need to complete to install and set up ShiftLeft in the [Dashboard](https://www.shiftleft.io/dashboard).

![Dashboard Instructions Page](/quickstarts/img/add-app.png)

You can always return to this page at a later date by clicking **Add App** in the Dashboard.

Begin by selecting the operating system you are using (in this case, select macOS); this will update the command-line code samples ShiftLeft provides you in the following steps.

Next, install the ShiftLeft CLI. You can do this in one of two ways:

1. Downloading the ShiftLeft CLI and manually adding the CLI to your system path.

2. Running the provided command-line command to download and adjust the permissions for the CLI:

```bash
$ curl https://cdn.shiftleft.io/download/sl > /usr/local/bin/sl && chmod a+rx /usr/local/bin/sl
```

Once you have the ShiftLeft CLI installed, you need to associate the CLI with your ShiftLeft account. The specific command you need is provided in the Dashboard, and it follows this format:

```bash
$ sl auth --org "YOUR_ORG_ID" --token "YOUR_ACCESS_TOKEN"
```

This step accomplishes two things: link the CLI running on your machine with your ShiftLeft account using ShiftLeft's API (the token included is needed to call the API).

## Inspect Your Code

At this point, you are ready to run Inspect. Please see the language-specific pages for details pertinent to your application:

* [C#](/inspect/analyzing-applications/c-sharp.md)
* [Go](/inspect/analyzing-applications/golang.md)
* [Java](/inspect/analyzing-applications/java.md)
* [Scala](/inspect/analyzing-applications/scala.md)
