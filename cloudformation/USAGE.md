# Deploying Fargate Self-Hosted Runners

- The stack will create an ECR repository for storing custom runner images that you will build with the Dockerfile under `_docker/`. Feel free to customize that image to include all the necessary test, build, compile, and packaging tools your Github Actions workflow will require.
- Runners must be assigned to either a specific organization or repository. This stack currently assigns them to a specific Github respository as specfied in the `GithubRepo` parameter
- The subnets specified in the `SubnetIds` parameter must belong to the same VPC as specified in the `VpcId` parameter. The subnets should be _private_ and allow access to the internet only through a NAT gateway.

## To deploy:

1. First deploy the template with 0 set for the NumRunners parameter. 
2. Build the docker image under the `_docker/` folder and push it to the ECR repo created by the stack.
4. On Github, generate a personal access token (PAT) with access to the necessary repositories then store that value in the Secrets Manager secret created by the stack. Token access must include the **workflow** scope.
```
aws secretsmanager put-secret-value --secret-id <secret name provided by stack output> --secret-string "<secret value>"
```
Note: be sure to protect your shell history! You can do this by storing the secret temporarily in a local file and using `$(cat <path/to/secret/file>)` instead of pasting the value directly.

5. Update the stack, set the `NumRunners` parameter to 1 or more, and the `ImageTag` parameter to the tag you applied to the image prior to pushing it to the ECR repo.
6. Go to Github > repo > Settings > Actions > Runners and note you should have runners registered

For additional consultation please visit us at https://www.aquia.us/
