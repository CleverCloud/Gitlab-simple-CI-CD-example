
# Deploy from GitLab to Clever Cloud

This is a simple example of CI/CD file to deploy from GitLab to Clever Cloud using [clever-tools](https://www.clever-cloud.com/doc/getting-started/cli/)

To try it, add the `.gitlab-ci.yml` file at the root of your project.

This script does the following: 

- download clever-tools (our CLI) Docker image
- [overrides the entry-point](https://docs.gitlab.com/ee/ci/docker/using_docker_images.html#override-the-entrypoint-of-an-image) to be able to execute `clever` + `<arguments>`
- applies a rule to only execute the workflow if we committed to the default branch (in the case of our project, it's `main`), which you can set in your GitLab project in **Settings > Repository > "Branch defaults"**
- deploys the commit on our production app

## Environment Variables

You need to set some variables in order to be able to use clever-tools in GitLab jobs. Go to your project's **Settings > CI/CD > "Variables"** and add the following variables:

- `CLEVER_TOKEN` AND `CLEVER_SECRET` (find it in your machine, usually in `~/.config/clever-cloud/clever-tools.json`)
- `APP_ID` to avoid committing it (find it at the top right in Clever Cloud Console, in your application tab).

The rest of variables in this script are implicit in GitLab.

More info can be found in [GitLab's doc](https://docs.gitlab.com/ee/topics/build_your_application.html)

## Troubleshooting

If you encounter the error

> Failed to push your source code because your repository is shallow and therefore cannot be pushed to the Clever Cloud remote

This means, GitLab make a [shallow clone](https://git-scm.com/docs/shallow) of the repository for the CI.
This basically means the repository has an incomplete history.

By default, GitLab uses a limited shallow clone as referenced here: <https://gitlab.com/gitlab-org/gitlab/-/issues/350100>

Solution is to make sure one has a full copy of the repository.  
To do so, in GitLab, go to `Settings` > `CI/CD` > `General pipelines` and scroll to the `Git strategy` section.  
There either choose `git clone` to have a full clone of the repository or use `git fetch` but set `Git shallow clone` to `0` to make sure you get the full history
