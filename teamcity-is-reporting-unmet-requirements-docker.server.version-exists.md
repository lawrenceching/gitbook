# TeamCity is reporting "Unmet requirements: docker.server.version exists"

TeamCity has build-in solution for Docker build. It requires you to install Docker in TeamCity agent. But sometimes I found some of Docker agents were not working.

```text
Unmet requirements: 
docker.server.version exists
```

There are several things to check for this issue:

1. Ensure Docker is installed properly in TeamCity agent server
2. Ensure Docker is accessible under the user TeamCity agent runs on
3. Ensure Docker is up and running, `docker version` is not enough, try `docker info`.



