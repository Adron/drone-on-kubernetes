# Drone on Google Container Engine

This directory contains various example deployments of Drone on [Google Container Engine](https://cloud.google.com/container-engine/).

**Note: While Drone supports a variety of different remotes, this demo assumes
that the projects you'll be building are on GitHub.**

## Prep work

**Before continuing on to one of the example setups below, you'll need to create a GKE cluster**, plus a persistent disk for the DB. Here's a rough run-down of that process:

### Create a Container Engine Cluster

There are a few different ways to create your cluster:

* If you don't have a strong preference, make sure your `gcloud` client is pointed at the GCP project you'd like the cluster created within. Next, run the `create-gke-cluster.sh` script in this directory. You'll end up with a cluster and a persistent disk for your DB. Your `gcloud` client will point `kubectl` at your new cluster for you.
* The Google Cloud Platform web console makes cluster creation very easy as well. See the [GKE docs](https://cloud.google.com/container-engine/docs/before-you-begin)), on how to go about this. You'll want to use an g1-small machine type or larger. If you create the cluster through the web console, you'll need to manually point your `kubectl` client at the cluster (via `gcloud container clusters get-credentials`).

### Create a persistent disk for your sqlite DB

By default, these manifests will store all Drone state on a Google Cloud persistent disk via sqlite. As a consequence, we need an empty persistent disk before running the installer.

You can either do this in the GCP web console or via the `gcloud` command. In the case of the latter, you can use our `create-disk.sh` script after you open it up and verify that the options make sense for you.

In either case, make sure the persistent disk is named `drone-server-sqlite-db`. Also make sure that it is in the same availability zone as the GKE cluster.

## Choose an example deploy

At this point you should have a cluster and a persistent disk (both in the same AZ). Time to get to the fun stuff! Here are your options:

* `gke-with-http` - This is the simplest, fastest way to start playing with Drone. It stands up a fully-functioning cluster served over un-encrypted HTTP. This is *not* what you want in a more permanent, production-grade setup. However, it is a quick and easy way to get playing with Drone with a bare minimum setup process.
* `gke-with-https` - Unlike `gke-with-http`, this example setup is ready for production. It uses nginx + Let's Encrypt to automatically generate and rotate SSL certs. There are a few more steps involved, but the included install script walks you through the whole process interactively. If you

## Stuck? Need help?

We've glossed over quite a few details, for the sake of brevity. If you have questions, post them to our [Help!](https://discuss.drone.io/c/help) category on the Drone Discussion site. If you'd like a more realtime option, visit our [Gitter room](https://gitter.im/drone/drone).
