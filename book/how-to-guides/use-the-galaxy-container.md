(galaxy-container)=
# How to use the QIIME 2 galaxy container

The QIIME 2 Galaxy docker container enables you to use QIIME 2 through the Galaxy interface on your own hardware.

We recommend using either [Podman](https://podman.io) or [Docker](https://docker.com).
Before you jump in with QIIME 2, follow the "Get Started" (i.e., install) instructions for one or the other on the project's website, and confirm that it's working according to the Podman's or Docker's instructions.
(We don't link to their instructions here so that we don't send you to an outdated link.)

```{admonition} Podman versus Docker
:class: dropdown, question
We don't take much of a stance on whether Podman or Docker is a better tool for using QIIME 2.

Podman seems to have some interesting benefits though, including ease of transition to paid cloud environments (via [Kubernetes](https://www.digitalocean.com/products/kubernetes)) if you need more computational resources than you have access to.
Podman also doesn't require that you have root/admin access on the computer where you're using it, so may work better if you're using computer hardware that is administered by others (such as a laptop computer owned and maintained by your employer).

You can find a perspective on the differences between the two from the developers of Podman [here](https://www.redhat.com/en/topics/containers/what-is-podman#podman-vs-docker).
```

(pull-galaxy-image)=
## Pulling and starting the container image

After downloading and learning to use your chosen program, run the following command in a terminal:

`````{tab-set}
````{tab-item} Docker
```shell
docker container run \
 -d \
 -p 8080:80 \
 -p 8021:21 \
 -p 8020:20 \
 quay.io/qiime2/<distribution>-galaxy:<epoch>
```
````
````{tab-item} Docker on Apple M-series
```shell
docker container run \
 -p 8080:80 \
 -p 8021:21 \
 -p 8020:20 \
 --platform "linux/amd64" \
 quay.io/qiime2/<distribution>-galaxy:<epoch>
```
````
````{tab-item} Podman
```shell
podman container run \
 -d \
 -p 8080:80 \
 -p 8021:21 \
 -p 8020:20 \
 --platform "linux/amd64" \
 quay.io/qiime2/<distribution>-galaxy:<epoch>
```
````
````{tab-item} Podman on Apple M-series
```shell
podman container run \
 -p 8080:80 \
 -p 8021:21 \
 -p 8020:20 \
 --platform "linux/amd64" \
 quay.io/qiime2/<distribution>-galaxy:<epoch>
```
````
`````

This will both pull and start the image. You will want to keep track of the name of the container created and use that name in place of `quay.io/...` for future invocations.

(use-container)=
## Using the container image

Open a web browser and navigate to localhost:8080. You will get a 502 page while Galaxy is booting. Within a few minutes you should be able to refresh and see the Galaxy page.

(build-the-galaxy-image)=
## Building the image locally (optional; experts only ♦♦)

**Expert users** may ultimately be interested in modifying the image used here.
This can be done with `docker` or `podman` as follows.
[Pulling the image](pull-galaxy-image) will be quicker and easier.

```{admonition} Podman Sudo
Podman may require you to use `sudo podman image build` which would require you to use `sudo podman container run` to actually use the image.
```

<!-- TODO: This will look something like the following, exact instructions dependent on final form of Dockerfile

1. Clone q2galaxy

2. Get an env.yml for the env you want to use in galaxy

3. Run q2galaxy's prepare.sh on that yaml

4. Run docker/podman build on the tools produced

 -->


<!-- TODO: Update this to be the real one when it exists -->
First, download the Dockerfile for the workshop container.
```shell
wget https://raw.githubusercontent.com/qiime2/q2galaxy/refs/heads/dev/docker/Dockerfile
```

Then, edit the file to specify the epoch (e.g., 2024.10) and distribution (e.g., `amplicon`) that you want to build your container for.

Next, make sure that the Docker daemon is running (e.g. by launching Docker Desktop).

Finally, build the image.

`````{tab-set}
````{tab-item} Docker
```shell
docker image build -t my-image-name .
```
````
````{tab-item} Docker on Apple M-series
```shell
docker image build -t my-image-name --platform "linux/amd64" .
```
````
````{tab-item} Podman
```shell
podman image build -t my-image-name .
```
````
````{tab-item} Podman on Apple M-series
```shell
podman image build -t my-image-name --platform "linux/amd64" .
```
````
`````

Note: This is also doable with Podman, but you may need to do `sudo podman image build` which may then require you to use `sudo podman` to use the image.
