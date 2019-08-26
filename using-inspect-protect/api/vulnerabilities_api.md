# Vulnerability API (Beta Version) for ShiftLeft Inspect and ShiftLeft Protect

You use the Vulnerability API to query for data on code vulnerabilities. A vulnerability is a potentially exploitable path in the code base. 

The API returns a list of vulnerabilities, by [organization](#listing-organizational-vulnerabilities) or [application version](#listing-vulnerabilities-of-an-application), with all the necessary information for you to take action on a code exploitable area. 

## Authenticating API Requests.

Any Public API request needs to be authenticated by adding a `Public API Token` to a header in the form of:

* Key: `Authorization`
* Value: `Bearer <your_token>`

To obtain the token, view your profile and copy the token supplied under the `Public API Token` section.

## Example of an API Return

```json
{
    "vulnerabilities": [
       {
           "applicationId": "project1549413585aa",
           "vulnerability": {
               "firstDetected": "041718202019.28",
               "vulnerabilityId": "database-write/e0ec210246d63ea44e28c01ed6113a66",
               "category": "a1-injection",
               "title": "Unsanitized database write in `DataLoader.run`",
               "description": "Attacker-controlled data is used in a database query without any sanitation or encoding. This could be intended behavior and thus has a low score. Injection flaws, such as SQL, NoSQL, OS, and LDAP injection, occur when untrusted data is sent to an interpreter as part of a command or query. By injecting hostile data, an attacker may trick the interpreter into executing unintended commands or accessing data without proper authorization which can result in data loss, corruption, or disclosure to unauthorized parties, loss of accountability, denial of access or even a complete host takeover.",
               "score": 1,
               "severity": "LOW_IMPACT",
               "securityEvents": "1",
               "blockedEvents": "1",
               "calls	": "1",
               "locationURL": "/account/{accountId}/addInterest",
               "locationMethod": "addInterest",
               "status": "FIXED",
               "assignedTo": "example@shiftleft.io",
               "dataFlow": <dataFlowObject>
           }
       }
       ],
       "nextPageBookmark": "eyJhbGciOiJSUzUxMiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1NTczNTE3NDAsImlhdCI6MTU1NzM1MDg0MCwiaXNzIjoiU2hpZnRMZWZ0Iiwib3JkZXJfYnlfZGlyZWN0aW9uIjoxLCJpbmNsdWRlX2NhbGxzIjp0cnVlLCJDcmVhdGVkQXRCb29rbWFyayI6MTU1NzM0NDc3OH0.VzENvpKuho5iu4KT5R285PC033RPBAaq6xoqFSXg4uJQp1OTElcBYN6EOtunfE3ByahJqgigMXKFxSQ6GIti8AogQc0uQwJvtl6D3HPGvFvpIKdMDu3I77LpoaSMJXIaT3sb03Js78pEnUw0JyMhLBcwXjiC2lycEEWb5TE4zvgP5HE7uwPDTtObJeFQjmQq2fcQOqXFL_PbU2aISA9qgE9OdmX2bqX1my0YcvM0GnaCdNwAddWmvAngoz7GvSW8Uozxeqrs6JXGmHrSGoF4QWvW8klnBv8V92o5CazJuiW_O1nvFK4Yw1mA3ywbtmp6hERYF5EJ_uhx3RXv2YR2dw"
       
```

where

**vulnerabilities**

* `applicationId`. Unique identification for each application within your organization.
* `vulnerability`
	* `firstDetected`. UNIX timestamp of the date and time when ShiftLeft first detected this vulnerability.
	* `vulnerabilityId`. Unique ID of this particular vulnerability in this application.
	* `category`. Vulnerability OWASP category.
	* `title`. Vulnerability title.
	* `description`. Vulnerability description.
	* `score`. Vulnerability float score, from 1-9, representing the OWASP severity.
	* `severity`. Either `LOW_IMPACT`, `MEDIUM_IMPACT`, `HIGH_IMPACT` representing the score level assigned by ShiftLeft.
	*  `securityEvents`. Count of security relevant events for the vulnerable endpoint.
	*  `blockedEvents`. Count of security relevant events that were blocked.
	*  `calls`. Total calls on the vulnerable endpoint. Represents regular traffic and any detected attacks.
	*  `locationURL`. Published location of the affected endpoint.
	*  `locationMethod`. Starting method of the vulnerable code path.
	*  `status`. UI configurable status of the vulnerability (ie. assignees, fixed or ignore state, etc.)
	*  `assignedTo`. Email address of individual assigned to fix or triage this vulnerability.
	*  `dataFlow`. Detailed information of the flow of the data triggering the vulnerability including origin function and target.

**nextPageBookmark**

* `nextPageBookmark`. String containing a bookmark to be passed to the same endpoint to obtain the next page of results, if any exists.
	

## Pagination

The ShiftLeft Vulnerability API endpoints are always paginated. Results are shown in sequential pages containing a maximum of 50 results. The results are displayed in three categories according to severity. Pagination information is provided with each response 
to access next page in the form of `nextPageBookmark` , each page has a life of 15 minutes.

```json
{
  	"vulnerabilities": [],
  	"nextPageBookmark": "eyJhbGciOiJSUzUxMiIsInR5cCI6IkpXVCJ...TGkubZyC69DRg",
 	"totalResults": "155",
  	"lowImpactResults": "104",
	"mediumImpactResults": "21",
	"highImpactResults": "30"
}
```

## Listing Organizational Vulnerabilities

You use the following request to return vulnerabilities by organization

POST `/api/v3/public/org/{organization id}/vulnerabilities/`

The result returns vulnerabilities with the following criteria:

* Belonging to the latest analyzed version of any app
* Of any severity
* Of any category
* With any description
* With or without calls
* With or without blocked calls
* Arbitrarily sorted
* Only the first page (50 results)

### Filtering an Organization's Vulnerabilities

You can filter returns in the body of the query

```json
{
"query": {
        "orderByDirection": "VULNERABILITY_ORDER_DIRECTION_DESC",
        "applicationId": [
            "project1549413585aa"
        ],
        "statusFilter": [
            "FIXED", "ASSIGNED"
        ],
        "severityFilter": [],
        "typeFilter": "",
        "assignedToFilter": [],
        "returnRuntimeData": true,
        "hasRuntimeData": true
    }
  "pageBookmark": "eyJhbGciOiJSUzUxMiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1NTczNTE3NDAsImlhdCI6MTU1NzM1MDg0MCwiaXNzIjoiU2hpZnRMZWZ0Iiwib3JkZXJfYnlfZGlyZWN0aW9uIjoxLCJpbmNsdWRlX2NhbGxzIjp0cnVlLCJDcmVhdGVkQXRCb29rbWFyayI6MTU1NzM0NDc3OH0.VzENvpKuho5iu4KT5R285PC033RPBAaq6xoqFSXg4uJQp1OTElcBYN6EOtunfE3ByahJqgigMXKFxSQ6GIti8AogQc0uQwJvtl6D3HPGvFvpIKdMDu3I77LpoaSMJXIaT3sb03Js78pEnUw0JyMhLBcwXjiC2lycEEWb5TE4zvgP5HE7uwPDTtObJeFQjmQq2fcQOqXFL_PbU2aISA9qgE9OdmX2bqX1my0YcvM0GnaCdNwAddWmvAngoz7GvSW8Uozxeqrs6JXGmHrSGoF4QWvW8klnBv8V92o5CazJuiW_O1nvFK4Yw1mA3ywbtmp6hERYF5EJ_uhx3RXv2YR2dw"
}
```

**Filter**

* `orderByDirection`. Either `VULNERABILITY_ORDER_DIRECTION_DESC` or `VULNERABILITY_ORDER_DIRECTION_ASC` is applied to the `firstDetected` field of a vulnerability.
* `applicationId`. If included, only returns results for these application IDs.
* `statusFilter`. If present, only returns results for vulnerabilities with these statuses.
* `severityFilter`. If present, only returns results for vulnerabilities of these severities.
* `typeFilter`. If present, the vulnerability human readable type is matched.
* `assignedToFilter`. If present, only returns vulnerabilities that have been assigned to the passed emails.
* `returnRuntimeData`. True by default. Indicates that `securityEvents`, `blockedEvents` and `calls` are returned as part of the results. Note that using these parameters can make the query return slower.
* `hasRuntimeData`. False by default. Indicates that only vulnerabilities whose endpoint has calls should be returned.

**Page Bookmark**

* `pageBookmark`: If present,`query` will be ignored and next page of the query indicated by this bookmark will be returned if it exists, this is optional.

## Listing Vulnerabilities of an Application

You use the following request to return vulnerabilities by an application

POST `/api/v3/public/org/{organization id}/app/{application id}/vulnerabilities/`

The result returns vulnerabilities with the following criteria:

* Belonging to the latest analyzed version this app.
* Of any severity
* Of any category
* With any description
* With or without calls
* With or without blocked calls
* Arbitrarily sorted
* Only the first page (50 results)

To obtain results for a specific application version, use 

POST `/api/v3/public/org/{organization id}/app/{application id}/version/{version}/vulnerabilities`


### Filtering an Application's Vulnerabilities

You can filter returns in the body of the query

```json
{
"query": {
        "orderByDirection": "VULNERABILITY_ORDER_DIRECTION_DESC",
        "statusFilter": [
            "FIXED", "ASSIGNED"
        ],
        "severityFilter": [],
        "titleFilter": "",
        "assignedToFilter": [],
        "returnRuntimeData": true,
        "hasRuntimeData": true,
        "vulnerabilityId": "database-write/e0ec210246d63ea44e28c01ed6113a66"
        },
        pageBookmark: ""
}
```

* `orderByDirection`. Either `VULNERABILITY_ORDER_DIRECTION_DESC` or `VULNERABILITY_ORDER_DIRECTION_ASC` is applied to the `firstDetected` field of a vulnerability.
* `applicationId`. If included, only returns results for these application IDs.
* `statusFilter`. If present, only returns results for vulnerabilities with these statuses.
* `severityFilter`. If present, only returns results for vulnerabilities of these severities.
* `titleFilter`. If present, the vulnerability title is partially-matched. Note that using this parameter can make the query return slower.
* `assignedToFilter`. If present, only returns vulnerabilities that have been assigned to the passed emails.
* `returnRuntimeData`. True by default. Indicates that `securityEvents`, `blockedEvents` and `calls` are returned as part of the results. Note that using these parameters can make the query return slower.
* `hasRuntimeData`. False by default. Indicates that only vulnerabilities whose endpoint has calls should be returned.
* `vulnerabilityId`. If present, only the vulnerability identified by it will be returned, this is a way to make a permalink to a vulnerability.

**Page Bookmark**

* `pageBookmark`: If present,`query` will be ignored and next page of the query indicated by this bookmark will be returned if it exists, this is optional.
