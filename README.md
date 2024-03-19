# nb-helper

The nb-helper repository contains a basic set up of Containerfiles and Github Actions to automatically trigger
container image builds when a notebook is added or changed under the **notebooks** directory.

The **base** directory contains a Containerfile that builds the **base image** we use for building our notebook
images. The **start-notebook.sh** is a script we need to add to the container image to ensure that jupyterlab runs
in the container when started. You will need to build this base image and push to the registry you use for your workflow.

The **common-notebook-setup** directory contains a Containerfile that we use to build the notebook image of the
notebooks that have been added or changed. The base image used here is the image built from the **base** directory.
Alternatively, you can use your own base image. Just update the value passed to **BASE_IMAGE**.

There are two github workflows configured here. The first one is **image_build_landing.yml** which is used to check
whether there was a change under the **notebooks** directory, all notebooks live under this directory and we only want
to trigger a new image build when there is a change in this directory. This action also allows us to build multiple images at the same time if many notebooks were updated in the same commit.

The second workflow is **build_notebook_image.yml** which builds new container images for any notebook changes. It then
tags the image with the **latest** tag as well as the sha of the commit - this helps to keep track of the changes going in.
The image is then pushed to the registry configured.

The workflows in this repository can be cloned and further configured to fit your needs!
