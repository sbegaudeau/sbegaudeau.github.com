---
layout: post
title:  "DevFest Nantes 2018"
subtitle: "Highlights from DevFest Nantes 2018"
author: "Stéphane Bégaudeau"
date: 2018-10-25 7:05:17 +0200
image: "/img/posts/2018/10/25/devfest-nantes-2018/devfest-preview.png"
include_obeo_rss: false
---

Last week, I had the pleasure to go back to DevFest Nantes and, just like last year, it was a great experience. It is the perfect opportunity to stay up to date on technologies such as Android, Firebase, Google Cloud Platform and the web in general.

<img src="{{ site.url }}/img/posts/2018/10/25/devfest-nantes-2018/devfest-banner.jpg" class="img-fluid img-border">

The DevFest was located once again at La Cité Nantes Congress Centre, a perfect place for such conference. The rooms are great, the sound is perfect and even if there are hundreds of persons, everything runs smoothly. The organization did a tremendous job along with the volunteers and the sponsors to create once again a great event.

<img src="{{ site.url }}/img/posts/2018/10/25/devfest-nantes-2018/keynote.jpg" class="img-fluid img-border">

The conference started with a keynote by [Chris Heilmann](https://twitter.com/codepo8) where he talked about our responsibilities as developers. He highlighted how we have to provide better tools and more robust solutions instead of trying to move from one trend to the other. How we should consider ourselves more like engineers than rockstars or artists.

## Google Cloud Platform & Firebase

Guillaume Laforge was there to present [the latest features](https://cloud.google.com/blog/products/gcp/bringing-the-best-of-serverless-to-you) available on Google Cloud Platform for serverless applications. Using an application that he built to share pictures, we could have an overview of the various services available to build serverless applications.

<img src="{{ site.url }}/img/posts/2018/10/25/devfest-nantes-2018/gcp.jpg" class="img-fluid img-border">

I really like the mix of various technologies and languages to build this application with not only Google App Engine and Google Cloud Functions but also [Google Cloud Vision](https://cloud.google.com/vision) to identify the elements visible on the pictures.

We also had a presentation by Juarez Filho who explained how he created a geolocation application using Firebase. Using the [Google Maps API](https://cloud.google.com/maps-platform), Google Cloud Functions, Firebase Hosting and even Angular it seemed very easy to build and deploy such applications.

## Angular

Since we are mentioning Angular, Wassim Chegham came to talk about how things are going on in the Angular ecosystem.

<img src="{{ site.url }}/img/posts/2018/10/25/devfest-nantes-2018/angular.jpg" class="img-fluid img-border">

Even if I tend to prefer React, this presentation was a great way to keep up with the new features coming from the angular community. From Bazel-based builds to Angular Ivy, the new renderer for Angular applications, it’s nice to see that Angular is still going strong.

## User Experience

I was really interesting in learning more about typography and accessibility and the presentation by Marie Guillaumet was a good opportunity to do so.

After an overview of the various groups of people who are concerned by this topic from those who have visual or physical disabilities to those who suffer from cognitive disabilities, it was great to have an overview of some accessibility issues encountered by each of those groups.

Marie also gave us some tips and tricks to improve the accessibility of our applications. You can find most of them in the [Web Content Accessibility Guidelines](https://www.w3.org/standards/techs/wcag#w3c_all).

<img src="{{ site.url }}/img/posts/2018/10/25/devfest-nantes-2018/accessibility.jpg" class="img-fluid img-border">

I really like the fact that the Chrome Devtools can now display some information regarding the [accessibility of our web applications](https://developers.google.com/web/tools/chrome-devtools/accessibility/reference). You can for example evaluate the contrast ratio of your colors very easily.

<a href="{{ site.url }}/img/posts/2018/10/25/devfest-nantes-2018/bad.jpg">
  <img src="{{ site.url }}/img/posts/2018/10/25/devfest-nantes-2018/bad_th.jpg" class="img-fluid">
</a>

## Android Things

I finally got to learn more about [Android Things](https://developer.android.com/things) thanks to Etienne Caron and his small rover. After some explanations on the creation of the rover which even includes 3d printed parts, Etienne showed us how to get started with Android Things.

<img src="{{ site.url }}/img/posts/2018/10/25/devfest-nantes-2018/ghost.jpg" class="img-fluid img-border">

It's been a long time since I tried to build something using Android but it was nice to see some familiar concepts running on such a small device. We could see a demo with the camera of the rover streaming what it was seeing and he even made some augmented reality demonstrations with menus floating above the rover.

## JavaScript & Machine Learning

I also had the opportunity to have a look at machine learning with JavaScript thanks Anta Aidara. In her talk, she showed how we could use Tensorflow with JavaScript thanks to [Tensorflow.js](https://js.tensorflow.org).

<img src="{{ site.url }}/img/posts/2018/10/25/devfest-nantes-2018/machinelearning.jpg" class="img-fluid img-border">

After a quick overview of all the concepts involved in machine learning, we could see how she has created a clone of Tinder powered by machine learning. In her application, she can train the ML model to recognize some gestures thanks to the camera of her laptop and then use this clone of Tinder only with her camera.

## Mind control in JavaScript

I also had a great time watching Charlie Gerard play with JavaScript and brain sensors to control an application with her mind. She gave us some nice explanation of the state of the art on brain sensors along with some demonstrations of a keyboard and even a drone controlled with her mind only.

<img src="{{ site.url }}/img/posts/2018/10/25/devfest-nantes-2018/brain.jpg" class="img-fluid img-border">

Even if possibilities are a bit limited now, it looks very cool and it's really impressive to see someone making things move just by thinking about it.

## JavaScript Event Loop

Benjamin Cavy made us dive into the internal parts of web browsers with a talk on the details of the JavaScript event loop. This talk was a good explanation of the way the stack and the various queues work together in a modern web browser to provide support for promises and other asynchronous features.

<img src="{{ site.url }}/img/posts/2018/10/25/devfest-nantes-2018/loop.jpg" class="img-fluid img-border">

It was really nice to see the various instructions move from the code to the stacks and the render queue in this slide to have a good feeling of the things are working. Such explanations should almost be mandatory for all web developers. If you are interested by this topic, have a look at [this article](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules) by Jake Archibald.

## Testing with Cypress

I have heard of [Cypress](https://www.cypress.io) multiple times but I did not had the opportunity to test it and, thanks to the talk of Rodolphe Bung, I have now seen it in action. It looks great and it seems to provide an amazing end-to-end testing experience.

<img src="{{ site.url }}/img/posts/2018/10/25/devfest-nantes-2018/cypress.jpg" class="img-fluid img-border">

There were lots of very impressive demos during the talk. Even if Cypress was only used with a simple example, it highlighted lots of its features and the amazing developer experience that it provides. I think I will try to use it in some future projects.

## Vanilla JavaScript

Matthieu Lux gave us his feedback on the state of some of the latest web technologies. As I suspected it, most of the features from ECMAScript 2015 like classes are now well supported in evergreen browsers. It was visible in his slides that we are pretty much only using babel to play with experimental features and to support old browsers like IE11.

<img src="{{ site.url }}/img/posts/2018/10/25/devfest-nantes-2018/vanilla.jpg" class="img-fluid img-border">

I really like his evaluation of HTTP/2 and more specifically the HTTP/2 push support. While it works today with modern browsers, determining what to push is still [very difficult](https://jakearchibald.com/2017/h2-push-tougher-than-i-thought). We could try to manually parse our HTML and JavaScript files to find out the dependencies required but it is very complex to setup. We are really missing something server-side to handle this issue right now.

It was also very interesting to have a look at the state of web components and some of [the issues](https://bugzilla.mozilla.org/show_bug.cgi?id=877072) surrounding HTML imports for example. Especially if you remember some of the issues with [the use of Shadow DOM](https://twitter.com/cpeterso/status/1021626510296285185) by Youtube a couple months ago.

## Building a great team

Sébastien Charrier gave a talk on how to build a great team. In his testimony, he walked us through various positive and negative situations he encountered in his professional life.

It really makes you think about what you are doing in your company to onboard new hires, to create and maintain a good work environment and to support remote work for example. It really resonates with me since over the years, we have worked hard on those subjects at [Obeo](https://www.obeo.fr) to build a great company culture.

As an example, you really need to take seriously your onboarding process to help newcomers feel welcome. What have you planned for their first day? Their first week? Have you thought about who is going to interact with them during this first week? Will they have everything needed to work? What about their training? Do you have a mentoring system in place? It really takes time and willingness to build and improve over time a great company culture.

## Security

Speaking about company culture, Goulwen Le Fur and Marc Lebrun gave us great explanations to integrate security concerns in your company's DNA. Starting by evaluating the state of the security of your applications thanks to penetration testing for example, we have seen how strict you have to be to make security a part of your development process.

It's a complex road ahead of all of us and it does not stop at a writing proper code. Have you audited your dependencies for vulnerabilities? Have you read their security recommendations? Are you keeping an eye on their CVEs? You really need to organize your planning and processes in order to take security into account.

## Conclusion

Finally, I have to mention the closing session with the french game Burger Quizz which was absolutely perfect. Everyone in the organization did a fantastic job for this game. The set, the details, the questions, we had a great time in the audience.

<img src="{{ site.url }}/img/posts/2018/10/25/devfest-nantes-2018/burger.jpg" class="img-fluid img-border">

This week I’m in Germany, as always this time of the year, for EclipseCon Europe 2018 to talk about web technologies such as [GraphQL](/2018/09/20/getting-started-with-graphql.html) that we have used this summer to build the new HTTP API of [Eclipse Sirius](https://www.eclipse.org/sirius). Don't hesitate to come and find me to talk about Siriuc and GraphQL. If you could not make it, you can still find me on [Twitter](https://www.twitter.com/sbegaudeau).