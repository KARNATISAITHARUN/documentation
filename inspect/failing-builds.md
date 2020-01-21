# How Fail Builds Based on Inspect Results

You can have builds in your CI/CD pipeline fail automatically based on Inspect's code analysis results. For example, you might want to fail a build that has:

* Exceeded the number of allowable vulnerabilities
* One or more instances of a specific type of vulnerability

## Step 1: Create Your Filter

[Create a filter](https://github.com/ShiftLeftSecurity/documentation/tree/480e92b3099a21c6d0c79f1399cfeea1a7853c19/inspect/using-dashboard/filter-results.md#creating-a-filter) that defines the specific vulnerabilities for which you want a build failed. [Save the filter](https://github.com/ShiftLeftSecurity/documentation/tree/480e92b3099a21c6d0c79f1399cfeea1a7853c19/inspect/using-dashboard/filter-results.md#saving-a-filter) and make sure that you check **Build Rule for Org** in the save filter prompt.

![Save Build Rule](../.gitbook/assets/build-rule.jpg)

## Step 2: Call the ShiftLeft API

Once you have your filter defined, call the ShiftLeft API to compare the Inspect results against your filter criteria.

The following script is one way to implement this functionality. It invokes ShiftLeft's public API to perform the comparison. If your code passes \(e.g., the results presented by Inspect indicate that there aren't sufficient vulnerabilities that match your build rule criteria\), the script returns `1` to indicate success. If not, the script returns `0` to indicate the build doesn't match your build rule criteria.

You will need to provide your the following values to the script \(all are available in the ShiftLeft Dashboard\):

* API public token
* Organization ID
* Application ID

```bash
#!/bin/sh

set -e

# Provide your API public token
PUBLIC_TOKEN=`cat token.txt`
# Provide your organization ID public token
ORGID=`YOUR_ORG_ID`
# Provide your application ID
APPID='YOUR_APP_ID'

# Assume this is the "tag_key=tag_value" use case "branch=branch_name"
TAG_KEY='branch'
TAG_VALUE=$1

SL_PROTO='https'
SL_HOST='www.shiftleft.io'
SL_URL="$SL_PROTO://$SL_HOST/api/v3/public/org/$ORGID/app/$APPID/tag/$TAG_KEY/$TAG_VALUE/build"
BEARER='Authorization: Bearer '$PUBLIC_TOKEN
BUILD_RESULT=$(curl --fail --show-error -X GET \
   $SL_URL \
  -H 'Accept: */*' \
  -H "$BEARER" \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H "Host: $SL_HOST" \
  -H 'accept-encoding: text/plain, deflate' \
  -H 'cache-control: no-cache' \
  -H 'cookie: Cookie_3=value' \
  -s -b Cookie_3=value)

echo $BUILD_RESULT

SUCCESS=$(echo $BUILD_RESULT | grep "success")
if [ -z "$SUCCESS" ]; then
    exit 1;
fi
exit 0
```

