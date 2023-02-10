---
title: Glue API guides
description: guides for developers working with the Spryker Glue API
originalLink: https://documentation.spryker.com/2023020/docs/glue-rest-api-new/index.html
template: glue-api-storefront-guide-template
originalArticleId: 62beddb6-7450-4b75-beef-6b11201456bf
redirect_from:
  - /docs/glue-rest-api
  - /docs/en/glue-rest-api
  - /docs/scos/dev/glue-api-guides/202200.0/glue-rest-api.html
---

This section contains a collection of guides for working with the Spryker Glue API:


[Introduction](/docs/scos/dev/glue-api-guides-new/202302.0/introduction.md)

Chapter 1: Introduction

Overview of the Glue API
    Purpose and benefits of using the Glue API
    High-level explanation of the API components and architecture
Chapter 2: Getting Started
    Setting up the development environment
    Authentication and authorization
    Making your first API call
    Handling errors and debugging
    Chapter 3: API Reference
Chapter 3: API Reference
    Detailed descriptions of the API endpoints and input/output parameters
    Error codes and how to handle them
    Chapter 4: Resources

Links to additional resources such as code samples, tutorials, and FAQs
    Information on how to get support and provide feedback
    Endpoint 1: [GET] /products

Retrieves a list of products from the Spryker framework.
    Input parameters: offset, limit, sort
    Output parameters: products, total
    Endpoint 2: [GET] /products/{productId}

Retrieves information about a specific product from the Spryker framework.
    Input parameters: productId
    Output parameters: product