# Install localhost gitlab-runner

Since Ubuntu "plunky" is not supported by the installer yet, installation is manual:

```bash
# Replace ${arch} with any of the supported architectures, e.g. amd64, arm, arm64
# A full list of architectures can be found here https://s3.dualstack.us-east-1.amazonaws.com/gitlab-runner-downloads/latest/index.html
curl -LJO "https://s3.dualstack.us-east-1.amazonaws.com/gitlab-runner-downloads/latest/deb/gitlab-runner-helper-images.deb"
curl -LJO "https://s3.dualstack.us-east-1.amazonaws.com/gitlab-runner-downloads/latest/deb/gitlab-runner_amd64.deb"

sudo dpkg -i gitlab-runner-helper-images.deb gitlab-runner_amd64.deb
```

# Register instance runner

```bash
./register-gitlab-runner.sh
```

# Create the Gitlab group and projects

Ref: ./README.md
