# Recipe


## To create a new recipe 
rad recipe register azure --environment default --template-kind bicep --template-path ghcr.io/radius-project/recipes/azure/rediscaches:latest --resource-type Applications.Datastores/redisCaches
rad recipe register customRedis --environment default --template-kind bicep --template-path ./customRecipe/redisCache.bicep  --resource-type Applications.Datastores/redisCaches

