---
layout: post
title: How to set onPush as the default change detection strategy for new Angular components?
date: 2020-09-07
excerpt: 
tags: [angular, change-detection, cli, onpush, schematics]
published: true
---

Most angular components I create are generated using the angular CLI. Every time I had to set the change detection to onPush after the component has been created. I know there is an option to set it as the default, but I was lazy to search and set it up.

Last week I was developing the overall architecture for a new module. I was generating a lot of skeleton components. It was frustrating to set onPush manually for every new component. I searched  how to set the onPush as the default strategy for new components.

Step 1 : Open your angular.json file.

Step 2: Find the section that says schematics, probably close to bottom.

```json
{
  "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
  "version": 1,
  "newProjectRoot": "projects",
  "projects": {...},
  "schematics": {
    "@schematics/angular:component": {
      "prefix": "app",
      "style": "less",
    },
  },
```

Step 3: Find the schematics configuration for angular components, named angular: components.

Step 4: Add the following under the component schematics section.

```json
"changeDetection": "OnPush"
```

That's it, any new components you generate will be configured to onPush change detection strategy. I regretted that I was lazy and was doing it manually for so long. There are a lot of other schematics available in the angular.json file. You can also use them as parameters for the ng generate command. Check them [here](https://angular.io/cli/generate).
