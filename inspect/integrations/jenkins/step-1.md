## How to Provide Your ShiftLeft Account Parameters to Jenkins

To integrate Inspect with Jenkins, you will need to create environment variables storing your ShiftLeft Organization ID and Access Token. Jenkins needs access to these parameters so that it can submit your code for analysis.

1. Log in to Jenkins with an administrator user's account.

2. Go to **Manage Jenkins** > **Configure System** > **Global properties**.

3. Check the **Environment variables** box and create the following two variables:

| **Name** | **Value** |
| - | - |
| `SHIFTLEFT_ORG_ID` | Organization ID |
| `SHIFTLEFT_ACCESS_TOKEN` | Access Token |

You can find your Organization ID and Access Token in the ShiftLeft Dashboard under [Account Settings](https://www.shiftleft.io/user/profile).

![Adding Jenkins environment variables for ShiftLeft authentication](/using-inspect-protect/integrating-with-shiftleft/img/jenkins-envvars.png)

Click **Add** to save.
