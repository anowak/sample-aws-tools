Sample for downloading AWS CLI tools
====================================

This sample demonstrates how to download the AWS tools, so subsequent builds will not get stuck on warnings regarding
overwriting the files.

Two approaches can be taken here:

1. Manually downloading the tools, ensuring that the operation is not performed on every build (to save time) and
that it will not fail due to some file being already present:

        before_archive:
          - >
            which aws || (
              wget -N https://s3.amazonaws.com/aws-cli/awscli-bundle.zip -P /tmp &&
              unzip /tmp/awscli-bundle.zip -d /tmp &&
              /tmp/awscli-bundle/install -b ~/bin/aws)

2. Installing the tools as a Python package.
As the Shippable userspace tools are using Python themselves, we use `virtualenv` here to provide isolation
of AWS CLI dependencies:

        before_archive:
          - virtualenv ve && source ve/bin/activate && pip install awscli

  Note that you don't need to use it for Python jobs (as `virtualenv` is already set up for them):

        before_archive:
          - pip install awscli

This sample is built for Shippable, a docker based continuous integration and deployment platform.
