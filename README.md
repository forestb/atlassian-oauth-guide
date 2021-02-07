atlassian-oauth-guide

- [Introduction](#introduction)
- [Guide](#guide)
- [Resources](#resources)
  - [Scopes](#scopes)
  - [Atlassian API Documentation](#atlassian-api-documentation)
  - [Atlassian + OAuth 2.0 (3LO)](#atlassian--oauth-20-3lo)

# Introduction
The intent is to make it easy to get a bearer token using OAuth 2.0 (3LO) for the Atlassian API.

The guide below outlines step-by-step instructions for (1) creating a Jira app, (2) using Postman to authorize a user with the necessary permissions to operate on that user, and then (3) making an API call to test it out.

This guide uses Postman variables where sensitive information should go. Always use environment variables in place of hard coding sensitive data.

# Guide
1. Create your Jira App
   1. Visit https://developer.atlassian.com/console/myapps/
   2. Use the "Create App" button
      1. With OAuth 2.0 (3LO) integration
   3. Once your app is created, go to "Permissions"
      1. Configure "Jira platform REST API"
         1. Configure your scopes; for script automation, I configured:
            1. `read:jira-work` (View Jira issue data)
            2. `write:jira-work` (Create and manage issues)
2. Authenticate and Authorize using Postman
   1. Create a new request
      1. `GET https://api.atlassian.com/oauth/token/accessible-resources`
      2. Configure the "Authorization" tab
         1. Left-hand side
            1. Type: `OAuth 2.0`
            2. Add authorization data to: `Request Headers`
         2. Right-hand side
            1. Configure New Token
               1. Token Name: `Atlassian OAuth 2 token`
               2. Grant Type: Authorization Code
               3. Callback URL: `https://oauth.pstmn.io/v1/callback` *Check box saying Authorize using browser*
               4. Auth URL: `https://auth.atlassian.com/authorize?audience=api.atlassian.com`
               5. Access Token URL: `https://auth.atlassian.com/oauth/token`
               6. Client ID: `{{ATLASSIAN_CLIENT_ID}}`
               7. Client Secret: `{{ATLASSIAN_SECRET}}`
               8. Scope: `read:jira-work write:jira-work` 
                  1. *Note: these should match the scopes you configured your app for in a previous step*
      3. Use "Get New Access Token" button; follow prompts
   2. Your bearer token is now available; use this going forward
      1. *Optional: Make a request to `https://api.atlassian.com/oauth/token/accessible-resources` with your bearer token to scopes.*
3. Use the Jira Rest API to GET an Issue
   1. Create a new request
      1. `GET https://{YOUR_ORGANIZATION}.atlassian.net/rest/api/3/issue/{ISSUE_KEY}`
      2. Configure the "Authorization" tab
         1. Left-hand side
            1. Type: `Bearer Token`
         2. Right-hand side
            1. Token: `{{ACCESS_TOKEN}}`

# Resources
## Scopes
As of writing this, the [scopes available](https://developer.atlassian.com/cloud/jira/platform/scopes-for-connect-and-oauth-2-3LO-apps/) are:
* `read:jira-user` `// View user profiles`
* `read:jira-work` `// View Jira issue data`
* `write:jira-work` `// Create and manage issues`
* `manage:jira-project` `// Manage project settings`
* `manage:jira-configuration` `// Manage Jira global settings`

## Atlassian API Documentation
* https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/
* https://developer.atlassian.com/cloud/jira/software/rest/intro/


## Atlassian + OAuth 2.0 (3LO)
* [OAuth 2.0 (3LO) apps](https://developer.atlassian.com/cloud/jira/platform/oauth-2-3lo-apps/)
* [Scopes for Connect and OAuth 2.0 (3LO) apps
](https://developer.atlassian.com/cloud/jira/platform/scopes-for-connect-and-oauth-2-3LO-apps/)