# Guide: Creating a GitHub Project via the API

## Introduction

This guide will walk you through the process of creating a new project within a GitHub organization using the GitHub GraphQL API. This is useful for automating project creation and integrating it into your existing workflows.

## Prerequisites

Before you begin, you will need the following:

*   **A GitHub Personal Access Token (PAT):** This token must have the `project` scope to create projects. For more information on creating a PAT, see the [official GitHub documentation](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).
*   **The Node ID of your GitHub organization:** You can find this by using the GitHub API.
*   **A command-line tool for making HTTP requests:** This guide will use `curl` for its examples.
*   **jq:** The complete example uses `jq` to parse the JSON response from the GitHub API. You can install it from the official `jq` [website](httpshttps://jqlang.github.io/jq/download/).

## Step-by-Step Instructions

### 1. Get the Node ID of Your Organization

To create a project within an organization, you first need to get the organization's `node_id`. You can do this by making a GET request to the following endpoint:

```bash
curl -H "Authorization: bearer YOUR_TOKEN" \
     -H "Accept: application/vnd.github.v3+json" \
     https://api.github.com/orgs/YOUR_ORGANIZATION
```

In the response, you will find a field called `node_id`. This is the value you will need for the next step.

### 2. Create the Project

Now that you have the `node_id` of your organization, you can create a new project by making a POST request to the GitHub GraphQL API.

Here is an example `curl` command that creates a new project named "My New Project":

```bash
curl --request POST \
  --url https://api.github.com/graphql \
  --header 'Authorization: bearer YOUR_TOKEN' \
  --data '{"query":"mutation {createProjectV2(input: {ownerId: \"YOUR_ORGANIZATION_NODE_ID\" title: \"My New Project\"}) {projectV2 {id}}}"}'
```

Make sure to replace the following placeholders:

*   `YOUR_TOKEN`: Your GitHub Personal Access Token.
*   `YOUR_ORGANIZATION_NODE_ID`: The `node_id` of your organization that you retrieved in the previous step.

If the request is successful, you will receive a response containing the `id` of the newly created project.

## Complete Example

Here is a complete, copy-and-paste example that you can use to create a new project. Remember to replace the placeholder values with your own.

```bash
# First, get the organization's node_id
ORG_NODE_ID=$(curl -H "Authorization: bearer YOUR_TOKEN" \
                  -H "Accept: application/vnd.github.v3+json" \
                  https://api.github.com/orgs/YOUR_ORGANIZATION | jq -r '.node_id')

# Then, create the project
curl --request POST \
  --url https://api.github.com/graphql \
  --header 'Authorization: bearer YOUR_TOKEN' \
  --data '{"query":"mutation {createProjectV2(input: {ownerId: \"'"$ORG_NODE_ID"'\" title: \"My New Project\"}) {projectV2 {id}}}"}'
```

## Conclusion

You have now successfully created a new project in your GitHub organization using the GitHub API. You can use this method to automate project creation and integrate it into your existing workflows. For more information, please see the official [GitHub API documentation](https://docs.github.com/en/graphql).
