# Concourse pipeline creation

## Repo setup

```bash
manifests/
	deployment.yaml
	version
	other_yaml_files.yaml
```

In the deployment file make sure the following are updated.

```yaml
image: harbor.guldentech.com/{rancher/harbor_project}/{repo_name}:TAG
```

```yaml
imagePullSecrets:
- name: guldentech-harbor-registry
```

This is only for non version controlled repos
```yamls
env:
- name: version
  value: THE_VERSION
```

## Creating the pipeline

```bash
# concourse-team is the same as rancher and harbor project
# local is the prod cluster
# c-m-48j66fz4 is the dev cluster

./fly -t guldentech set-pipeline \
	-p {repo_name} \
	-c build-deploy.yaml \
	--team={concourse-team} \
	-v git_user={git_user} \
	-v repo_name={repo_name} \
	-v branch={branch} \
	-v project={rancher/harbor_project} \
	-v cluster=local/c-m-48j66fz4 \
	-v email={notification_email} \
	-v email_password={gmail_password}
```

## Delete pipeline

```bash
# concourse-team is the same as rancher and harbor project

fly -t guldentech destroy-pipeline \
	-p {repo_name} \
	--team={concourse-team}
```
