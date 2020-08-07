filefrog/stunnel
===============

This image wraps up the `stunnel` in a small-ish Docker image, for
use in plumbing Kubernetes of Docker-based deployments.

[See it on Docker Hub!][1]

To get the most out of this container, you will need to mount in
your certificates, and a suitable `/etc/stunnel/stunnel.conf`
file.  This image really cannot stand on its own; it will most
likely find use as a container in a K8s Pod for something that
cannot or will not properly talk TLS to another thing.

Such an example, for Kubernetes, can be found in
deploy/k8s/redis-tls.yml.

After you apply that (it needs a namespace called `stunnel`,
first), you can check on the client for proof of functionality,
via:

    kubectl logs \
      $(kubectl get pod -l app=client
                        -o jsonpath='{.items[0].metadata.name}') \
      -c client -f

A similar example, for Docker (using Docker Compose) can be found
in deploy/docker/redis-tls/docker-compose.yml.  That one is best
run with a foreground `docker-compose up` from the `redis-tls/`
directory (so you get all the logs).

Building (and Publishing) to Docker Hub
---------------------------------------

The Makefile handles building pushing.  For jhunt's:

    make push

Is all that's needed for release.  If you want to build it
locally, you can instead use:

    make build

If you want to tag it to your own Dockerhub username:

    IMAGE=you-at-dockerhub/stunnel make build push

By default, the image is tagged `latest`.  You can supply your own
tag via the `TAG` environment variable:

   IMAGE=... TAG=$(date +%Y%m%d%H%M%S) make build push

Happy Hacking!


[1]: https://hub.docker.com/r/filefrog/stunnel
