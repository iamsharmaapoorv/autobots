# Hosting the site on GitHub Pages

The site is the `site` folder at the top of the repository. It holds `index.html` and is published as-is, with no build step.

## Turn it on

1. Save the workflow as `.github/workflows/deploy-pages.yml` on the `main` branch. The folder is `.github`, the subfolder is `workflows`, and the file name must match exactly.
2. In the repository, open **Settings**, then **Pages**. Under **Build and deployment**, set **Source** to **GitHub Actions**. Do not pick a branch — the workflow uploads the `site` folder itself.
3. Set the source before the first run. If the workflow runs while the source is still "Deploy from a branch", the deploy step stops with an error, and you can rerun it after changing the setting.
4. Push a commit to `main`, or open **Actions**, pick **Deploy static site to GitHub Pages**, and click **Run workflow** to start it by hand.
5. Check that it worked in two places. On the run page the `deploy` job shows a green check and the deployment step prints the site URL; under **Settings**, **Pages**, the same URL appears once the first deployment finishes.
6. If the run waits for approval, open it, approve the `github-pages` deployment, and it continues.

## Custom domain

1. In **Settings**, **Pages**, under **Custom domain**, enter `example.com` and save. For a `www` domain, enter `www.example.com`.
2. At the registrar where the domain is registered, create the DNS record GitHub shows on that page:
   - Apex domain `example.com`: four `A` records pointing to the addresses GitHub lists.
   - Subdomain `www.example.com`: a `CNAME` record pointing to `OWNER.github.io`.
3. Wait for the DNS check to pass, then enable **Enforce HTTPS**. The certificate is issued only after the record resolves.
4. You register and pay for the domain yourself, at a registrar of your choice. This repository does not create or renew it, and the site keeps working at its `github.io` address until the domain resolves.

## Roll back

1. In the repository, open the commit history of `main` and find the commit that changed the site.
2. Revert it with `git revert <commit-sha>`, or open a pull request that restores the previous version of the changed files.
3. Push the result to `main`.
4. The push starts the workflow again and republishes the `site` folder as it now stands.
5. Confirm the deploy job passed and open the site URL to see the earlier version.
