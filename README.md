# ocp-aux

### Deploy the AUX

The AUX installer requires that nodes be labeled to deploy the components if you are using local-storage. If you are using a different storage class, be sure to update that in the ../RECIPE_EXAMPLE/AUX/example_recipe.yaml before running the install.

```
patches/apply-aux-patch.sh
dep/bin/deploy-ric-aux dep/RECIPE_EXAMPLE/AUX/example_recipe.yaml
```

## Undeploy the AUX

dep/bin/undeploy-ric-aux
