# pre-commit-gitleaks-example

Demonstrates the difference in behavior between a go based pre-commit hook and docker-image based hook

### Reproduction

- Clone this repository and make sure to initialize pre-commit with `pre-commit install`
- Make a change that will trigger a detection in example.py (changing a few characters in the fake AWS_SECRET_ACCESS_KEY should be sufficient)
- Run `git commit -am 'test'`
  - Notice that the first hook fails (go) but the second hook passes (docker)
- Run `git add .; git commit -m 'test'`
  - In this case notice that both hooks fail
 
The difference is that each hook is looking for staged files, but the docker image does not receive the GIT_INDEX_FILE environment variable and is operating off of `.git/index` instead of the `.git/index.lock` file that git commits when run with the `--all` option.
