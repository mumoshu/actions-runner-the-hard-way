Run the runner container:

```
$ docker run -it --rm summerwind/actions-runner:v2.273.4 sh

# cd /runner
```

Browse to `https://github.com/USER/REPO/settings/actions`, then click `Add Runner` button in the top-right of the `Self-hosted runners` pane.

Configure the runner by running the below command shown in after you've clicked the button:

```
$ ./config.sh --url https://github.com/mumoshu/actions-runner-the-hard-way --token THE_AUTOMATICALLY_GENERATED_TOKEN
```

Finally, run any of:

- `run.sh`
- `run.sh --once` if you'd like it to stop after first job run
- `bin/runsvc.sh` if you'd like to keep it running by tolerating any retriable errors
- `bin/runsvc.sh --once` if you'd like it to be kept running after automated updates, while still wanting to stop after first job run.
  - You need [this patch](https://github.com/summerwind/actions-runner-controller/pull/99#issuecomment-695891631) for this to work

Voila! You can now trigger workflow runs to be run on this runner.

Git-commit-push the following workflow definition and watch out:

```
name: Go
on: [push, pull_request]
jobs:

  build:
    name: Build
    runs-on: self-hosted
    steps:

    - name: Set up Go 1.13
      uses: actions/setup-go@v1
      with:
        go-version: 1.13
      id: go

    - name: Check out code into the Go module directory
      uses: actions/checkout@v1

    - name: Test
      run: echo Test passed.
```


