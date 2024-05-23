# Recipe


## To create a new recipe 
1. Create a bicep file and define your recipe
2. Publish it 
`rad bicep publish --file myrecipe.bicep --target br:ghcr.io/USERNAME/recipes/myrecipe:1.1.0`
eg: `rad bicep publish --file ./customRecipe/redisCache.bicep --target br:github.intuit.com/ablaise/rad-recipes/rediscache:0.0.1`
3. Register it `rad recipe register customRedis --environment default --template-kind bicep --template-path br:github.intuit.com/ablaise/rad-recipes/rediscache:0.0.1  --resource-type Applications.Datastores/redisCache`


### To publish it locally
1. Start your local docker registry with `docker start registry`
2. rad bicep publish --file ./customRecipe/redisCache.bicep --target br:localhost:5000/rediscache:0.0.1 --plain-http



## Known errors
1. docker login docker.intuit.com -u  austin_blaise@intuit.com -p <API KEY>
    Error response from daemon: Get "https://docker.intuit.com/v2/": unknown: Login has failed. Due to Incorrect username/password or locked user.
    For some reason docker intuit doesnt wanna log me in with the API key
