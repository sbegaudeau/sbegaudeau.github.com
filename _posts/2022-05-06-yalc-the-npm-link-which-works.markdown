---
layout: post
title:  "Yalc, the npm link which works"
subtitle: "Easily manage npm packages locally"
author: "Stéphane Bégaudeau"
date: 2022-05-05 12:35:47 +0200
image: "/img/posts/2022/05/06/yalc-the-npm-link-which-works/app-first-version.png"
include_obeo_rss: false
---
If you want to maintain a couple of npm packages that depend on one another, you want to quickly test new features introduced, in one of your libraries, inside of your application.
You could of course create a new feature in one of your libraries, then push the code to Github, let another contributor review it, merge it, built it, and then perform a new release to consume this new release in your other applications but that's way too long and cumbersome.
You want to test everything locally first by consuming development versions of your libraries in your applications before releasing anything in the wild.

### A better npm link

You could use `npm link` to create symbolic links to your dependencies but it comes with a lot of constraints and corner cases and you can never be sure that things are working properly.
I tried it in the past and stumbled on tons of issues with it.

You could also switch to a monorepo approach with all your packages in a single repository with tools like Lerna, Turborepo, or Nx but they were a bit overkill for our simple need and we would have to change the way our code is organized to use them efficiently.
Looking for an alternative, my team and I decided to try using [Yalc](https://www.npmjs.com/package/yalc).
Yalc has been created as a better alternative to `npm link` by giving you a robust way to test development versions of your libraries in your applications without having to release anything on npm.
It does not do much more than that but that's ok since it does it pretty well.

<a href="https://www.npmjs.com/package/yalc" target="_blank" rel="noopener noreferrer">
<img src="{{ site.url }}/img/posts/2022/05/06/yalc-the-npm-link-which-works/yalc.png" class="img-fluid">
</a>

You can think of Yalc as having a package registry locally on your computer.
You can publish a new package to this registry anytime your want and then consume it from another package on your machine very easily.

To get started, you just need to install Yalc globally using `npm i yalc -g`.

### Publishing and consuming your library

Let's consider two packages named `lib` and `app` respectively.
You can create your library using any tool you want and, when you are ready to test it, you can publish it on your Yalc repository using `yalc publish`.
Yalc will then inform you that a new version has been successfully published:

```
sbegaudeau@sbegaudeau-macbook-pro lib % yalc publish
lib@0.0.1+9989ef9f published in store.
```

Now in your application, you can start using Yalc with `yalc add lib` to tell Yalc that you want to use the library published locally:

```
sbegaudeau@sbegaudeau-macbook-pro app % yalc add lib
Package lib@0.0.1+9989ef9f added ==> XXX/yalc-sample/packages/app/node_modules/lib.
```

After that, you can see in your package.json that the reference to the library has been modified from `"lib": "*"` to `"lib": "file:.yalc/lib"`.
This way, you can know very quickly which dependencies are being retrieved from Yalc instead of the npm registry.
You can also ask Yalc to show which packages are using which dependencies published with `yalc installations show`:

```
sbegaudeau@sbegaudeau-macbook-pro app % yalc installations show lib
Installations of package lib:
  XXX/yalc-sample/packages/app
```

After that, you can start your application, with `npm start` in our example, and leverage the code from your library as if it was downloaded from the npm registry.

<a href="{{ site.url }}/img/posts/2022/05/06/yalc-the-npm-link-which-works/app-first-version.png" target="_blank" rel="noopener noreferrer">
<img src="{{ site.url }}/img/posts/2022/05/06/yalc-the-npm-link-which-works/app-first-version_th.png" class="img-fluid img-border">
</a>

### Updating your library and applications

We have previously seen how to publish a library and consume it in our application.
Now let's see how we can improve our library and consume a new version using the same `yalc update` command.

<a href="{{ site.url }}/img/posts/2022/05/06/yalc-the-npm-link-which-works/publishing-the-library.png" target="_blank" rel="noopener noreferrer">
<img src="{{ site.url }}/img/posts/2022/05/06/yalc-the-npm-link-which-works/publishing-the-library_th.png" class="img-fluid">
</a>

To consume this new version in your application, you just have to run `yalc update lib` to update the dependency from your application.
After our call to `yalc add`, Yalc has created a lockfile named `yalc.lock` in our application folder.
It contains the details of the various dependencies that are managed by Yalc for our application.
Using `yalc update`, we can simply update to the latest version of our library published on Yalc.

```
sbegaudeau@sbegaudeau-macbook-pro app % yalc update lib
Package lib@0.0.1+e33da7c7 added ==> XXX/yalc-sample/packages/app/node_modules/lib.
```

You can now restart our application using `npm start` to see the updated library in action

<a href="{{ site.url }}/img/posts/2022/05/06/yalc-the-npm-link-which-works/app-second-version.png" target="_blank" rel="noopener noreferrer">
<img src="{{ site.url }}/img/posts/2022/05/06/yalc-the-npm-link-which-works/app-second-version_th.png" class="img-fluid img-border">
</a>

### Cleaning everything

Once you are done, you can remove the dependency to the locally installed version from your application using `yalc remove lib` to restore the dependency to its original state.
The file `yalk.lock` keeps the original dependency range and restores it when we use `yalc remove`.

```
{
  "version": "v1",
  "packages": {
    "lib": {
      "signature": "e33da7c713d5e5de436abdf4ab9f9ba3",
      "file": true,
      "replaced": "*"
    }
  }
}
```

```
sbegaudeau@sbegaudeau-macbook-pro app % yalc remove lib
Removing installation of lib in XXX/yalc-sample/packages/app
```

After that, in the `package.json`, the reference to the library goes back to `"lib": "*"` as it was before we started using Yalc.
If you want to remove the library published in the local yalc registry, you can simply use `yalc installations clean lib`.
You are now back to the original state as if nothing ever happened.

### Yalc in action

I have been using Yalc daily over the past two years to manage dependencies between three packages without issues.
It's a small tool, only used to provide a better workflow than `npm link` and it works.
It's very easy to set up and get started so don't hesitate to try it if you are maintaining a couple of packages that depend on each other and you don't want to switch to a more complex solution based on a monorepo.

If you want to test it, I have made the code used in this post available on [Github](https://github.com/sbegaudeau/yalc-sample).
If you have any additional questions, don't hesitate to contact me on [Twitter](https://www.twitter.com/sbegaudeau)