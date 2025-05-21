(galaxy-container)=
# How to use the QIIME 2 Galaxy container

The QIIME 2 Galaxy container enables you to use QIIME 2 through the Galaxy interface on your own hardware.

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
## Pulling the image and starting a container for the first time

After downloading and learning to use your chosen program, run the following command in a terminal:

`````{tab-set}
````{tab-item} Docker
```shell
docker container run \
 -d \
 -p 8080:80 \
 -p 8021:21 \
 -p 8020:20 \
 quay.io/qiime2/q2galaxy:<epoch>
```
````
````{tab-item} Docker on Apple M-series
```shell
docker container run \
 -p 8080:80 \
 -p 8021:21 \
 -p 8020:20 \
 --platform "linux/amd64" \
 quay.io/qiime2/q2galaxy:<epoch>
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
 quay.io/qiime2/q2galaxy:<epoch>
```
````
````{tab-item} Podman on Apple M-series
```shell
podman container run \
 -p 8080:80 \
 -p 8021:21 \
 -p 8020:20 \
 --platform "linux/amd64" \
 quay.io/qiime2/q2galaxy:<epoch>
```
````
`````

You will need to replace `<distribution>` and `<epoch>` with the distribution and epoch you are targeting. The first time you run this command it will both pull the image and run a container off of it. Every subsequent time it will see that you already have the image and will create a new container without pulling it again.

```{admonition} Subsequent runs of the container
Your Galaxy history will be saved in the container you are using, so if you want to resume from where you left off make sure to run the same container again instead of creating a new one. To do this, you will need to get the id of your container. How to do this is dependent on whether you are using `docker` or `podman` and how you are using them. Please consult the documentation for your chosen tool to get the id of your container. Once you have your container id, run the above command but swap out `quay.io/qiime2/q2galaxy:<epoch>` out with the container id.
```

(use-container)=
## Using the container

Once your container is running, open a web browser and navigate to localhost:8080. You will get a 502 page while Galaxy is booting. Within a few minutes you should be able to refresh and see the Galaxy page. At this point you can use Galaxy as normal and your Galaxy History will be saved in the container you are running.
