---
layout: post
title:  "Managing microservices with Open Shift"
subtitle: "A modern platform to deploy your services"
author: "Stéphane Bégaudeau"
date: 2019-02-07 10:12:17 +0200
image: "/img/posts/2019/02/07/managing-microservices-with-open-shift/preview.png"
include_obeo_rss: false
---
<img src="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/logo.jpg" class="img-fluid">

This week I had the pleasure to participate in a Red Hat workshop on microservices and [Open Shift](https://www.openshift.com) in Paris. Open Shift is an open source platform to manage microservice-based applications deployed in containers and orchestrated by Kubernetes. I had the opportunity to see some Open Shift demonstrations in the past but it was the first time I could really use it by myself.

<a href="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/codeready-workspaces-empty.png">
<img src="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/codeready-workspaces-empty_th.png" class="img-fluid">
</a>

For this workshop, we also used a brand new Red Hat product named [Red Hat CodeReady Workspaces](https://developers.redhat.com/products/codeready-workspaces/overview/). I was happy to discover that CodeReady Workspaces is in fact the Red Hat product built on top of the open source cloud-based IDE [Eclipse Che](https://www.eclipse.org/che/).

<img src="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/eclipse-che.png" class="img-fluid">

Since I am contributing to [multiple projects](https://accounts.eclipse.org/users/sbegaudeau#tab-projects) in the Eclipse Foundation, I have followed quite closely the development of Eclipse Che over the past few years. I even worked on an Eclipse Che extension to integrate [Eclipse Sirius](https://www.eclipse.org/sirius/).

<a href="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/sirius-che.png">
<img src="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/sirius-che.png" class="img-fluid img-border">
</a>

I have used Eclipse Che for a couple of minutes in the past but I never tried to use it for real as my main development environment so I was very existed by this opportunity. During one day, I've used Eclipse Che to build, deploy and debug microservices and it was a pleasure. Nothing to install or configure on my computer, I just had to open CodeReady Workspaces in my browser, select an already configured tooling configuration with everything to create microservices and I was ready to go in a couple of seconds.

<a href="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/codeready-workspaces-workspace.png">
<img src="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/codeready-workspaces-workspace_th.png" class="img-fluid">
</a>

After that I could start working on my Spring and Node services without having to leave my web browser.

<a href="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/codeready-workspaces-spring.png">
<img src="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/codeready-workspaces-spring_th.png" class="img-fluid">
</a>

From our instance of CodeReady Workspaces, we could connect to an Open Shift instance created for this workshop and deploy our microservices using the [Fabric8 Maven plugin](https://maven.fabric8.io). From there, we could open the Open Shift dashboard to have a look at our microservices.

<a href="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/open-shift-dashboard.png">
<img src="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/open-shift-dashboard_th.png" class="img-fluid">
</a>

I was a bit lost in the beginning with all the menus, pages and information available in Open Shift but after a couple of minutes, it is quite easy to find everything you need.  After a tour of Open Shift during which we have seen how to scale manually the number of instances of our services, we had a look at the integration of liveness and readyness probes to monitor our application.

<a href="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/open-shift-dashboard-2.png">
<img src="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/open-shift-dashboard-2_th.png" class="img-fluid">
</a>

Open Shift was then watching for issues with our services and relaunch them quickly if needed. It was also fun to kill instances on purpose and see Open Shift keep the whole application up and running by starting new ones instantanously.

<a href="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/open-shift-kill.png">
<img src="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/open-shift-kill_th.png" class="img-fluid img-border">
</a>

We also configured autoscaling rules to let Open Shift add or remove instances depending on the CPU and memory usage. In order to test those autoscaling rules, we used a load testing utility named Siege to quickly simulate some activity. I don't know the configuration of the infrastructure hosting the Open Shift instance used but I was a bit sad (yet relieved) to see that I could not make it sweat by spamming it for a couple of minutes. This traffic was still enough to trigger our autoscaling rules and Open Shift started new instances of some of our services to handle the load. Of course,when I stopped spamming the server, Open Shift automatically destroyed those instances too.

<a href="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/open-shift-autoscaling.png">
<img src="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/open-shift-autoscaling_th.png" class="img-fluid img-border">
</a>

Open Shift also integrates a Jenkins-based continuous integration platform which can be connected to a Git repository very easily. In a couple clicks, we could push new commits to our repository and see Jenkins build the new version of our service and deploy it automatically. Thanks to this integration, we could update one of the services of our application without effort.

<a href="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/open-shift-pipelines.png">
<img src="{{ site.url }}/img/posts/2019/02/07/managing-microservices-with-open-shift/open-shift-pipelines_th.png" class="img-fluid img-border">
</a>

Open Shift also gave us access to a set of preconfigured services such as Postgres databases which we deployed and connected to our application in a couple of minutes. We also tried some remote debugging of the services and databases from our CodeReady instances. We just had to setup a breakpoint, send a request to our HTTP endpoint and we were ready to debug our service.

In the end, I was really impressed by the Open Shift platform which gave me everything I needed to help me deploy and manage my microservices. The team from Red Hat which helped us during this workshop was also great with lots of explanation and technical details on the way Open Shift work.