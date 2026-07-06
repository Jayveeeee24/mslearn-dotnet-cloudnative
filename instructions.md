# Dev Container issues and fixes

# rename a container not the default
"runArgs": [
        "--name", "dotnet-compliance-project-container"
],

# if devcontainer doesn't build and docker is the issue
"ghcr.io/devcontainers/features/docker-from-docker:1": {},

# insert in json so that project container will use local ssh config
"mounts": [
        // Mounts your Windows SSH folder to the 'vscode' user's Linux SSH directory
        "source=${localEnv:USERPROFILE}/.ssh,target=/home/vscode/.ssh,type=bind,consistency=cached"
    ],
    "remoteEnv": {
        // Forwards your active local Windows SSH Agent socket down to the container
        "SSH_AUTH_SOCK": "${localEnv:SSH_AUTH_SOCK}"
    },