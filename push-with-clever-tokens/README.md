# Use Git in CI/CD without SSH key

Adding an dSSH key to variables in a GitLab repository is generally a bad idea, since GitLab can't mask it in the UI or in the workflow. An SSH key leak is a high security risk. If you still want to push on Clever Cloud using Git, you can use a `CLEVER_TOKEN` and `CLEVER_SECRET` pair.

### Variables you need to define

Add these variables in your repository:

- `CLEVER_TOKEN` and `CLEVER_SECRET`
- `APP_ID`
- `HOST` (for example: `push.par.clever-cloud.com` for apps hosted in Paris region)