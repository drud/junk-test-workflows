# Test out pull_request and pull_request_target for differences

This repo was just built to fully understand what's going on with the pull_request and pull_request_target events.

With `pull_request`, the code is checked out from the PR and the workflow is defined by the PR.

With `pull_request_target`, the code is checked out from upstream (the PR target, upstream/main) and the workflow is defined by the upstream code. In addition, some secrets like GITHUB_TOKEN are available. Checking out code when running against pull_request_target is highly discouraged.

* In the main branch you see that value.txt is "HEADvalue", and in pr1 branch, it's "PR1value".
* When you push to a PR two workflows are created, one for `pull_request` and one for `pull_request_target`. These aren't clearly distinguished in the [actions tab](https://github.com/drud/junk-test-workflows/actions) except that the `pull_request` one is labeled with `rfay:pr1`
* Example, a push to pr1 creates [pull_request](https://github.com/drud/junk-test-workflows/actions/runs/2290997766) and [pull_request_target](https://github.com/drud/junk-test-workflows/actions/runs/2290997743)
* If you look at the actual workflows, you'll see that [pull_request](https://github.com/drud/junk-test-workflows/runs/6343642397?check_suite_focus=true) has shows value.txt as the one in the PR. It also shows that it's using the workflow from the PR - "RUNNING WORKFLOW FROM PR". The [pull_request_target](https://github.com/drud/junk-test-workflows/runs/6343642341?check_suite_focus=true) workflow shows that it's got the value.txt from the upstream/main branch and is running "RUNNING WORKFLOW FROM UPSTREAM".

The docs on all this don't seem very clear to me, but they do strongly say not to check out code when using `pull_request_target`. See [docs](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#pull_request_target) and [article](https://securitylab.github.com/research/github-actions-preventing-pwn-requests). They all say "running in the context of the target repository". I guess that an evil PR could push to the upstream repo, which is the essence of the problem.

Additional info: [GitHub Actions Security Best Practices](https://blog.gitguardian.com/github-actions-security-cheat-sheet/)

