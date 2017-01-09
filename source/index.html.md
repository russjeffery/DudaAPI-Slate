---
title: Duda REST API Documentation

language_tabs:
  - shell: cURL

toc_footers:
  - <a href='#'>Back to Developer Portal</a>
  - <a href='#'>Duda API Terms of Service</a>
  - <a href='https://github.com/lord/slate' target="_blank">Documentation Powered by Slate</a>

includes:
  - authentication
  - access
  - errors
  - multiscreen
  - mobile
  - accounts
  - permissions
  - analytics

search: true
---

# Introduction

Welcome to the Duda API REST Documentation. Here you will find APIs for Mobile, Multiscreen (Responsive), Analytics and account management. Duda's REST API is designed to be a management API, which means you can integrate Duda's White Labeled resources (Sites, Accounts, etc..) into your own workflows or services. It is not currently designed to manage individual edits to websites.

Duda's platform has two primary website products: Mobile and Responsive. In the documentation here, the responsive platform, also called Multiscreen, while the Mobile is simply called mobile. Responsive is the primary website builder product that Duda customers use today, while mobile is our legacy product.

The API uses standard RESTful principles to manage resources:

* POST verb/method will create or update a resource
* GET verb/method will read an existing resource
* DELETE verb/method will delete an existing resource


