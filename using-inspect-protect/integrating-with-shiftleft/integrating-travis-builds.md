# Integrating Travis Builds

This section describes how to integrate Travis builds with ShiftLeft. 

## Travis Integration Prerequisites

To integrate Travis builds with ShiftLeft, please adhere to the following prerequisites:

- [Travis instance](https://travis-ci.org/) (hosted or on-prem) 
- Supported application and build tool (see [code analysis requirements](../../introduction/requirements.md))
- Familiarity with [ShiftLeft Inspect and Protect](../../using-inspect-protect/inspect-protect-quick-start.md)
- ShiftLeft account credentials: **Organization ID** and **Access Token**
Initially these credentials will be provided to you by ShiftLeft. Once you have established your account you can copy them from the **My Profile** page at the ShiftLeft Dashboard.

![Get ShiftLeft Account Credentials](img/copy-org.png)

## Travis Integration Instructions

You have a couple of options for integrating Travis builds with ShiftLeft. The first option is applicable to both editions of Travis: hosted and on-prem. The second option can only be used with Travis Enterprise (on-prem).

### Option 1: Configure the `travis.yml` file

The typical approach is to configure the CLI installation and `sl analyze` using the `travis.yml` file, which means you can use either the hosted or on-prem edition of Travis.

Here is an example `travis.yml` file that demonstrates how to integrate ShiftLeft with Travis:

```yaml
language: java
dist: trusty

install:
  - <INSTALL CLI TOOL> 
  - <any other dependency install steps>
after_install:
  - <RUN CLI & AUTHENTICATE (if not using environment variables, see note below)>
script:
 - run your tests here
 - or any other tasks
after_script:
 - <RUN CLI & EXECUTE COMMAND sl analyze> 
```

See [SL Auth](../using-cli/authenticating.md) for more information on using environment variables for authentication. See also the [Travis documentation](https://docs.travis-ci.com/user/environment-variables#Default-Environment-Variables).

### Option 2: Customize the container

Each Travis build uses an ephemeral Linux container (Docker). If desired you could modify the build containers to do the `install` (CLI installation) and `after_install` (`analyze`) steps. This involves editing the Dockerfile as described in the [Travis documentation](https://docs.travis-ci.com/user/enterprise/build-images/#Customizing-build-images).

> #### Note
>
> Modifying the build container is a Travis Enterprise (on-prem) feature only; you cannot modify the build container using hosted Travis.
