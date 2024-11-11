
# AI Center ML Skills Management

This repository contains examples of API requests to stop and resume AI Center ML Skills on UiPath's AI Center. The requests use the `curl` command and are designed to authenticate and manage ML skills in UiPath's staging environment.

## Prerequisites

- **API Access**: You must have access credentials (Client ID and Client Secret) for your UiPath AI Center environment. Read out more at [External Applications (OAuth)](https://docs.uipath.com/automation-cloud/automation-cloud/latest/api-guide/accessing-uipath-resources-using-external-applications) on how to get your API Access.
- **Authorization**: Ensure that your API token has the necessary scopes to create, view, edit, and delete ML skills (`AIC.MLSkills.Create`, `AIC.MLSkills.View`, `AIC.MLSkills.Edit`, `AIC.MLSkills.Delete`).

## Authentication

To authenticate, obtain a Bearer token using the following request:

```bash
curl --location 'https://staging.uipath.com/identity_/connect/token' \
--data-urlencode 'grant_type=client_credentials' \
--data-urlencode 'client_id={{client_id}}' \
--data-urlencode 'client_secret={{client_secret}}' \
--data-urlencode 'scope=AIC.MLSkills.Create AIC.MLSkills.View AIC.MLSkills.Edit AIC.MLSkills.Delete'
```

Replace `{{client_id}}` and `{{client_secret}}` with your actual credentials. The response will include a token that can be used to authorize subsequent requests.

## Retrieve ML Skills

List your ML Skills with this request:

```bash
curl --location 'https://staging.uipath.com/universe/Earth/aifabric_/ai-deployer/v1/mlskills' \
--header 'Authorization: Bearer {{access_token}}'
```

Replace `{{access_token}}` with the token obtained in the authentication step.

## Resume an ML Skill

To resume a paused ML Skill, use the following request, replacing the `ml_skill_id` with your specific skill ID:

```bash
curl --location 'https://staging.uipath.com/universe/Earth/aifabric_/ai-deployer/v1/mlskills/{{ml_skill_id}}?updateType=RESUME' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{access_token}}' \
--data '{
    "processor": "CPU",
    "deploymentsRequired": 1,
    "gpuRequired": 0,
    "publicSkill": false,
    "autoUpdate": false,
    "inactivityPeriodInDays": -1,
    "replicas": 1,
    "requestedCPU": 1.0,
    "requestedMemory": 4
}'
```

Replace `{{ml_skill_id}}` with your skill's unique ID, and `{{access_token}}` with the authorization token.

### Parameters

- **processor**: Type of processor (`CPU` or `GPU`).
- **deploymentsRequired**: Number of deployments needed.
- **gpuRequired**: Number of GPUs required (set to 0 if not needed).
- **publicSkill**: Boolean indicating if the skill should be public.
- **autoUpdate**: Boolean for auto-updating the skill.
- **replicas**: Number of replicas to deploy.
- **requestedCPU**: CPU units required.
- **requestedMemory**: Memory in GB required.

## Stop an ML Skill

To stop an ML Skill, use this request:

```bash
curl --location --request PUT 'https://staging.uipath.com/universe/Earth/aifabric_/ai-deployer/v1/mlskills/stop/{{ml_skill_id}}' \
--header 'Authorization: Bearer {{access_token}}'
```

Replace `{{ml_skill_id}}` with the ML Skill ID and `{{access_token}}` with the Bearer token.

## Notes

- The `ml_skill_id` is a unique identifier for each ML Skill. Replace it with the actual ID for your skill.
- The `access_token` expires after a certain time. Re-authenticate as needed.
- Ensure the necessary permissions are set for the actions you intend to perform.
  
For further information, refer to [UiPath's API documentation](https://docs.uipath.com/) or contact your UiPath Technical Account Manager.
