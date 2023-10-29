# Install Docker binaries into container
Place into your DOckerfile if you need to run docker commands inside your container
```
ENV DOCKERVERSION=20.0.6
RUN curl -fsSLO https://download.docker.com/linux/static/stable/x86_64/docker-${DOCKERVERSION}.tgz \
  && tar xzvf docker-${DOCKERVERSION}.tgz --strip 1 \
                 -C /usr/local/bin docker/docker \
  && rm docker-${DOCKERVERSION}.tgz
  ```