# kubectl for AWS eks 🚀
Provide the kubectl command to make actions on your eks cluster.

## Env check list
✓ `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` – **required**: aws credentials with eks access.

✓ `KUBE_CONFIG_DATA` – **required**: A base64-encoded kubeconfig file with configs for kubernetes to access the cluster. 
>You can get it by running the following command:
```bash
cat $HOME/.kube/config | base64
```

✓ `AWS_DEFAULT_REGION` – **optional**: If we do not define, the region from the `KUBE_CONFIG_DATA` will be used.

✓ `KUBE_NAMESPACE` and `KUBE_APP_NAME` – **optional**: To exemplify a rollout restart flow.

## Usage

`.github/workflows/eks.yml`

```yml
on: push
name: deploy
jobs:
  deploy:
    name: Kubectl eks deploy to cluster
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@main
    - name: Rollout restart
      uses: jonathan-sh/kubectl@1
      env:
        kube_confg_data: ${{ secrets.KUBE_CONFIG_DATA }}
        kube_namespace: ${{ secrets.KUBE_NAMESPACE }}
        kube_app_name: ${{ secrets.KUBE_APP_NAME}}
        aws_access_key_id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws_secret_access_key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws_region: ${{ secrets.AWS_DEFAULT_REGION }}
      with:
        args: '"kubectl -n $kube_namespace rollout restart $kube_app_name"'
```