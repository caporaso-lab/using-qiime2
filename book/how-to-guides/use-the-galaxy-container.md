(galaxy-container)=
# How to use the QIIME 2 galaxy container

The QIIME 2 galaxy docker container enables you to use QIIME 2 through the galaxy interface on your own hardware.

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

(pull-image)=
## Pulling and starting the container image

After downloading and learning to use your chosen program, run the following command in a terminal (if using `podman` simply swap `podman` in for `docker` in the command):

```shell
docker run -d \
 -p 8080:80 \
 -p 8021:21 \
 -p 8020:20 \
 quay.io/qiime2/<distribution>-galaxy:<epoch>
```

This will both pull and start the container.

(use-container)=
## Using the container image

Open a web browser and navigate to localhost:8080. You will get a 502 page while galaxy is booting. Within a few minutes you should be able to refresh and see the galaxy page.
