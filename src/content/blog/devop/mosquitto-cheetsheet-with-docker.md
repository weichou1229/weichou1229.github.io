---
author: WeiChou
pubDatetime: 2023-10-02T00:00:00Z
modDatetime: 2023-10-02T00:00:00Z
title: The Mosquitto cheat sheet with Docker
slug: mosquitto-cheatsheet-with-docker
featured: true
draft: false
tags:
  - mosquitto
  - MQTT
  - docker
description:
  The Cheat Sheet for running the eclipse-mosquitto MQTT Broker with docker.
---

## Table of contents

## Deploy a Mosquitto broker with docker
```
docker run -d --rm --name mqtt-broker -p 1883:1883 eclipse-mosquitto:1.6
```

## Subscribe
```
docker run --rm -it eclipse-mosquitto:1.6 mosquitto_sub -v -h 172.17.0.1 -t '#'
```

## Publish
```
docker run --rm -it eclipse-mosquitto:1.6 mosquitto_pub \
  -h 172.17.0.1 -t my-topic -m '{
  "msg": "hello world"
 }'
```

## Subscribe by using current MQTT broker
```
docker exec -it mqtt-broker mosquitto_sub -v -t "#"
```

## Publish by using current MQTT broker
```
docker exec -it mqtt-broker mosquitto_pub \
  -t my-topic -m '{
  "msg": "hello world"
 }'
```