---
layout: post
title: Why I used only immutable cattle server ?
---

To respond to this question, I should start with a little story.

When I started a few years ago with dedicated server, I used the web interface to select and order my server, enter my credit card information and after some minute I receved my ssh credential.

That was pretty cool, I could install softwares, setup the firewall, etc. But after some time, I find a new provider with best server setup at lower price. How can I migrate my server, my so cute pet server.

Let’s take a minute to clearly define pets and cattle. 

## Understanding Pets and Cattle

This explanation of Pets vs Cattle is what [resonated with Tim Bell at CERN](https://twitter.com/noggin143/status/354666097691205633) and many others and caused them to replicate and propagate the analogy, which created the meme that has edified so many and has so cleanly represented the transition we are all going through to cloud.


### Pets

Servers or server pairs that are treated as indispensable or unique systems that can never be down. Typically they are manually built, managed, and “hand fed”. Examples include mainframes, solitary servers, HA loadbalancers/firewalls (active/active or active/passive), database systems designed as master/slave (active/passive), and so on.

### Cattle

Arrays of more than two servers, that are built using automated tools, and are designed for failure, where no one, two, or even three servers are irreplaceable. Typically, during failure events no human intervention is required as the array exhibits attributes of “routing around failures” by restarting failed servers or replicating data through strategies like triple replication or erasure coding. Examples include web server arrays, multi-master datastores such as Cassandra clusters, multiple racks of gear put together in clusters, and just about anything that is load-balanced and multi-master.

## Time to automate was coming

In truth it was not so easy, I started with [Capistrano](http://capistranorb.com) a remote server automation and deployment tool written in Ruby. This had the advantage of being in a git repository and I can recreate from scratch the server. So when I need to make some change I used the provider console to reset the server and recreate it.

## Mutable Infrastructure vs Immutable Infrastructure

Configuration management tools such as Chef, Puppet, Ansible, and SaltStack typically default to a mutable infrastructure paradigm. For example, if you tell Chef to install a new version of OpenSSL, it’ll run the software update on your existing servers and the changes will happen in-place. Over time, as you apply more and more updates, each server builds up a unique history of changes. This often leads to a phenomenon known as configuration drift, where each server becomes slightly different than all the others, leading to subtle configuration bugs that are difficult to diagnose and nearly impossible to reproduce.

The Configuration management tools are not dead, they have completely their place to provision immutable server, but maybe the complicated part of what is the state of my server will be replaced with Immutable provisioner like CoreOS with cloud-init or packer to automate the setup of an immutable image.

## The last mile

To complete the last mile we need to have the full infrastructure as code. Over a cloud provider that define his resources between API. Common cloud provider like AWS, Azure, Google Cloud, Openstack, ...

I use Terraform for all the good reason in the article [Why we use Terraform and not Chef, Puppet, Ansible, SaltStack, or CloudFormation](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c)

I finish to completely create an etcd cluster of immutable cattle server with coreOS, terraform and openstack.

In my next post, I'll describe 
- how to automate change in Ubuntu distribution image with packer.
- CoreOS Container Linux how to setup clout-init with Ignition.
- Bootstrap CoreOS kubernetes with terraform on Openstack.

Author: [Stéphane Cusin](https://github.com/quidio)