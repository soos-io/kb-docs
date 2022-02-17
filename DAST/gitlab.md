---
title: Integrating SOOS DAST with GitLab CI
gistURL: https://gist.github.com/soostech/7b74eb66cc1bde6cc4506eb67538fc14
---

# How to Integrate SOOS DAST with your GitLab CI

<img src="../assets/img/SOOS-Icon.png" alt="SOOS" width="128" height="128">
<img src="../assets/img/gitlab.png" alt="GitLab" width="128" height="128">

This document will take you step-by-step through the tasks required to set up a GitLab repo, for scan it with the SOOS DAST Product.
## Prerequisites

- You need to have a SOOS Account.
- You need to have a GitLab repo.

## Steps

<summary class='section-title'>Create or Update your <code>.gitlab-ci.yml</code> file</summary>

Once the `SOOS_CLIENT_ID` and `SOOS_API_KEY` have been configured, the next step is create or update the `.gitlab-ci.yml` file.

**Note:** You need to create a `.gitlab-ci.yml` file in your repository root directory if you don't have it.

1. Copy the content below in your `.gitlab-ci.yml` file to run the `SOOS DAST Analysis`


<script src="https://gist.github.com/soostech/7b74eb66cc1bde6cc4506eb67538fc14.js"></script>

