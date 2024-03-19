
# Deploy review apps from GitLab to Clever Cloud

This is an example of CI/CD file to deploy a review app from GitLab to Clever Cloud using [clever-tools](https://www.clever-cloud.com/doc/getting-started/cli/)

To try it, add the `.gitlab-ci.yml` file at the root of your project.

This script does the following:

1. Download clever-tools (our CLI) Docker image
2. [Overrides the entry-point](https://docs.gitlab.com/ee/ci/docker/using_docker_images.html#override-the-entrypoint-of-an-image) to be able to execute `clever` + `<arguments>`
3. Create a new app in Clever Cloud from the branch the Merge Request is opened on
4. Update the review app on every new commit
5. Delete the review app
6. Deploy to production fro every new commit on the default branch (in the case of our project, it's `main`), which you can set in your GitLab project in **Settings > Repository > "Branch defaults"**

Since GitLab doesn't provide an easy way to detect whether a MR is closed or opened, jobs are manual by default. Trigger them directly from the MR UI.

![Manual jobs from Merge Requests UI](/assets/gitlab-manual-jobs.png)

## Environment Variables

You need to set some variables in order to be able to use clever-tools in GitLab jobs. Go to your project's **Settings > CI/CD > "Variables"** and add the following variables:

- `CLEVER_TOKEN` AND `CLEVER_SECRET` (find it in your machine, usually in `~/.config/clever-cloud/clever-tools.json`)
- `APP_ID` to avoid committing it (find it at the top right in Clever Cloud Console, in your application tab).

Add the variables your app uses as well. Check all available environment variables in Clever Cloud [in the documentation](https://developers.clever-cloud.com/doc/reference/reference-environment-variables/).

More info can be found in [GitLab's doc](https://docs.gitlab.com/ee/topics/build_your_application.html)
