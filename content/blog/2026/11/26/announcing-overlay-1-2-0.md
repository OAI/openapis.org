---
title: OpenAPI Overlay v1.2.0 Released
date: "2026-11-26"
author: lornajane
category: blog
tags: [overlay, openapi]
permalink: /blog/2026/11/26/announcing-overlay-1-2-0
---

There's a new version of the OpenAPI Overlay specification available, with two really key features included in this version: reusable actions, and the `$self` keyword. Let's look at each of those in more detail and how they may fit your existing use of Overlays.

## Reusable Actions

One piece of feedback we heard from Overlay users is that if an update operation doesn't neatly match a JSONPath, the Overlay document itself can end up very long and repetitive. This resonates with me since I have one use case where I was adding an extension to operations that started with a verb which is not very easy to express in JSONPath! For a simple operation like adding an extension, it's not an emergency, but we were seeing examples from the community of pretty deeply nested updates that needed to be repeated. And so reusable actions were born.

The first change you'll notice in an Overlay 1.2 description is that it now includes a `components:` section, like OpenAPI has. That's where the action is described, then it's referenced using `$ref` in any location where that action should be reused. An Overlay might end up looking like this:

```yaml
overlay: 1.2.0
info:
  title: Date formatting matters
  version: 1.0.0
components:
  actions:
    dateFormat:
      description: Mark date fields as date fields
      fields:
        update:
          format: date
actions:
  # Add date fields where they belong
  - target: "$.components.schemas['Performance'].properties['performanceDate']"
    $ref: '#/components/actions/dateFormat'
  - target: "$.components.schemas['Booking'].properties['dateBooked']"
    $ref: '#/components/actions/dateFormat'
```

When you apply this Overlay to an OpenAPI description with schemas that match the targets, the reusable action gets applied to each one. For this specific example it would make just as much sense to be spelled out in full, but with multiple matched targets and more complex update operations, this feature lends itself to the scale we're seeing in today's APIs.

Overlays are a key tool for building spec pipelines and features like this one add very valuable tools to our API transforms toolbox. As we quest for more and better ways to produce API descriptions for agents, for integrations, for other automations ... it all helps.

## The `$self` Keyword

Adding `$self` to the OpenAPI Overlay Specification for v1.2 follows the introduction of the same feature in OpenAPI v3.2. The main advantages are that it gives the document an identity beyond the URL it is served from. That URL and the `$self` field can be the same value, but if they're not then `$self` provides a base that other references are resolved from, including the `extends` field and other URIs. This technical detail unlocks a few multi-platform and development-pipeline use cases that were difficult or annoying without it.

## Upgrade to OpenAPI Overlay v1.2

There's an upgrade guide available on our documentation site with some great examples: https://learn.openapis.org/upgrading/overlay-v1.1-to-v1.2.html

Check with your favorite tooling vendors whether they already have support for Overlays 1.2, or when you can expect it. This helps them know you're interested!

## OpenAPI Overlays Today

The Overlays project is under active development. The features in this release were mostly the work of Vincent Biret, so a huge thank you to him for all the hard work and the MANY iterations that we went through before getting these features into their final version.

Overlays solve a problem that every API project has: their OpenAPI description could be better than it is, or more appropriate for a specific use case. The continued evolution of this standard helps keep putting the right tools in the hands of those that need them, and gives us more ways to adapt and improve even as the API landscape changes around us. To get involved with the project, join us on slack, check out the GitHub repo for the Overlays project, or join one of the bi-weekly open meetings for this project.