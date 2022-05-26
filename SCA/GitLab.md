# How to Integrate SOOS SCA with your GitLab CI
<div>
<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/gitlab.png" alt="GitLab" width="128" height="128">
</div>

This document will walk you through, step-by-step, how to set up a GitLab repository and scan it with the SOOS SCA Product.

## Prerequisites
- You need to have a [SOOS account](https://app.soos.io/register).
- You need to have a GitLab repository with a [supported](https://kb.soos.io/help/soos-languages-supported) manifest file.

## Steps
* When viewing your project, navigate to `Settings`, then to `CI/CD`, and expand the `Variables` section.
* Using the `Client Id` and `API Key` values found on the [GitLab Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=gitlab), create environment variables named `SOOS_CLIENT_ID` and `SOOS_API_KEY` for those values, respectively. These variables will be used by the SOOS CLI.
* Copy the contents of the [`gitlab_sca.yml`](https://gist.github.com/soostech/18ee324e95c234e3a0b2416eb1538feb) file, as seen on the [GitLab Integration page of the SOOS App](https://app.soos.io/integrate/sca?id=gitlab), to your `.gitlab-ci.yml` file.

## Run It
To run the SOOS CLI against your repository, just execute a build or commit a change.
