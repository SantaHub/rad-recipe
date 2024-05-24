# Recipe


## Authorize with github packages 
Need to use githubs container registry. could use any other, but i couldnt get others to work.
Follow instructions here for github.com, intuit's github doesnt allow you to host packages.
https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry#authenticating-with-a-personal-access-token-classic 

## To create a new recipe 
1. Create a bicep file and define your recipe
2. Publish it `rad bicep publish --file myrecipe.bicep --target br:ghcr.io/USERNAME/recipes/myrecipe:1.1.0`
eg: `rad bicep publish --file ./customRecipe/redisCache.bicep --target br:ghcr.io/ablaise/rad-recipes/redis:0.0.1`
3. Register it `rad recipe register re --environment default --resource-type Applications.Datastores/redisCaches --template-kind bicep --template-path localhost:5000/rediscache:0.0.1`

### To publish it locally (manifest file issues.)
1. install k3d. `brew install k3d`
2. create a registry `k3d cluster create --agents 2 -p "80:80@loadbalancer" --k3s-arg "--disable=traefik@server:0" --registry-create reciperegistry:51351`

1. write out the recipe in a bicep file
2. Publish the receipe `rad bicep publish --file ./customRecipe/redisCache.bicep --target br:localhost:51351/recipes/local-dev/rediscache:0.0.1 --plain-http`
3. Register the receipe `rad recipe register myredis --environment default --template-kind bicep --template-path 'localhost:51351/recipes/local-dev/rediscache:0.0.1' --resource-type   'Applications.Datastores/redisCaches' --plain-http`
"message": "code RecipeLanguageFailure: err failed to fetch repository from the path \"localhost:51351/recipes/local-dev/rediscache:0.0.1\": Head \"http://localhost:51351/v2/recipes/local-dev/rediscache/manifests/0.0.1\": dial tcp 127.0.0.1:51351: connect: connection refused",

4. Use the recipe in the app.bicep
```
resource db 'Applications.Datastores/redisCaches@2023-10-01-preview' = {
  name: 'db'
  properties: {
    environment: environment
    application: application
    recipe: {
      name: 'myrecipe' ##
    }
  }
}
```
5. deploy the app `rad deploy ./app.bicep`
This is failing with manifest fetch failures.

## Known errors
1. docker login docker.intuit.com -u  austin_blaise@intuit.com -p <API KEY>
    Error response from daemon: Get "https://docker.intuit.com/v2/": unknown: Login has failed. Due to Incorrect username/password or locked user.
    For some reason docker intuit doesnt wanna log me in with the API key
