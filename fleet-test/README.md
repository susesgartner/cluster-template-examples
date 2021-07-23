# rke2 cluster template

Helm chart that is hard coded to use Digital Ocean. User can select name and secret only, all else can only be configured from this repo.

### how to use

```bash
helm install --namespace fleet-local --value ./your-values.yaml my-cluster ./charts
```

General cluster options are available through [values.yaml](./values.yaml)

For different cloud provider drivers:

[Amazonec2](./values-aws.yaml)

[Vsphere](./values-vsphere.yaml)

[Digitalocean](./values-do.yaml)

[Azure](./values-azure.yaml)


### how to update this template

* make changes to appropriate files (cluster.yaml, values.yaml, questions.yaml, etc.)
* update the version number in Chart.yaml (or, if adding a new template, give it a new name and version)
* run `helm package charts && helm repo index ./ `
* make a PR for the new .tgz package along with the index.yaml file
