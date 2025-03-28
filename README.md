# Fast Synapse Deploy

## Overview
This action will deploy Azure Synapse artifacts using the publish branch.

### Major Features
 - Optimized for speed using connection pooling and multiple async requests to the Synapse API.
 - Leverages Azure CLI for authentication.
 - Supports HTTP_PROXY, HTTPS_PROXY, and NO_PROXY environment variables.
 - Can be combined with `validate` from [Microsoft action](https://github.com/marketplace/actions/synapse-workspace-deployment) (see note below).

## Pre-requisites for the action
Requires [Azure Login Action](https://github.com/marketplace/actions/azure-login).

## Not Supported 
 - Deploying ManagedPrivateEndpoints.
 - Deploying *directly* from branch other than `publish`. 
   - However, can be combined with `validate` action (see note below) to achieve same result. 
 - Incremental deployment. Hopefully not needed given the speed optimizations!

## Should I use this action?
 - If the [official Microsoft action](https://github.com/marketplace/actions/synapse-workspace-deployment) doesn't meet your needs, give it a try. This action allows for more async requests, resulting in a faster deployment. 

## Combine with the Microsoft 'Validate' Action
This action can work in tandem with the Microsoft `validate` action, which is required to address a [known issue](https://learn.microsoft.com/en-us/azure/synapse-analytics/cicd/continuous-integration-delivery#1-publish-failed-workspace-arm-file-is-more-than-20-mb) with file size limit during publish. High level, you would first use `validate` to generate the required templates, then use this action to quickly deploy from those templates. 

## SYNAPSE_API_LIMIT
This action seeks to deploy artifacts as quickly as possible. To that end, it will make multiple concurrent requests to the Synapse API. Depending on usage patterns (multiple and/or frequent deployments, for example), the Synapse API might respond with `TooManyRequests [429]`. The `SYNAPSE_API_LIMIT` environment variable is used to limit the number of concurrent requests to try and mitigate this issue.  Unfortunately, the Synapse API does not implement a `retry-after` header, so the entire deploy must be terminated.

## Review Me
Please consider [leaving a review](https://marketplace.visualstudio.com/items?itemName=shawn-mcgough.fast-synapse-deploy&ssr=false#review-details). I love to hear how it is helping with deployments!
At the end of the log there is duration information:
```
Completed deploy in 00:01:16.2082239.
Completed delete in 00:00:32.3423195.
```

## Release Notes

 - 1.0.12
   - Initial public release








