# advertising-mail

`advertising-mail` is the nickname for our notification delivery system. Upon interaction with the WiiGrindr channel, a POST request over the SDPY HTTP-like protocol is made. Packets are sent in big endian order.
Internally, a mixture of Vagrant, Docker, and bare metal is used.

The infrastructure is extremely simple:

  - A RabbitMQ instance hosted in an Ubuntu 18.04 image handles requests proxied by a Golang server and nginx in Docker.
  - From RabbitMQ, an internal XMPP instance connects to Prometheus for internal monitoring.
  - Another Docker image provides scaleable Redis, MySQL, MariaDB, etc images.
  - Bare metal hosts the entire thing on ramfs.

  Upon a received notification, your Wii eventually downloads the notification and delivers via exposed apache2.

Building is extremely simple likewise, following the KISS principal:

```
cd main
gradle build
cd ../server
npm install
cd ../server-backend
bundle install
cd ../server-exposed
yarn install
cd ../creation-system
go get
go build
cd ../images
docker-compose up docker-compose.yml
docker-compose up temp-workaround.yml
kubectl create -f undocumented-pod-routine.yml
vagrant up
cd temp
vagrant up
cd ../RabbitMQ
vagrant up
```

Estimated build time ranges 2-3 days.
