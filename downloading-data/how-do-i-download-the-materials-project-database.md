---
description: Methods for downloading data from the Materials Project (MP) database.
---

# How do I download the Materials Project database?

The Materials Project is an open resource, with data freely available for all. This includes making its data as easy to download as possible including in bulk. Typically, downloading data manually can be slow and tedious, so we provide an API [(Application Programming Interface)](https://en.wikipedia.org/wiki/API) so that data can be downloaded using popular programming languages.

The Materials Project API defines a standardized manner in which the Materials Project database can be accessed by its users. The API is typically used by scientific researchers who need to retrieve large amounts of data to support their research.

For technical users, this is a RESTful API, meaning that it uses standard HTTP methods for communication, while conforming to the [representational state transfer](https://en.wikipedia.org/wiki/Representational_state_transfer) architecture style of software.

The Materials Project offers two APIs:

* **The main API**, giving access to all data primarily generated or maintained by the Materials Project.
* **The** [**MPContribs API**](../mpcontribs.md), which gives access to user-contributed data included on the Materials Project.

To use the APIs, you can use any client that can make HTTP requests (e.g. GET, POST). However, we maintain an official Python client package (`mp-api`). **This is the best way to interact with the Materials Project API.**

{% content-ref url="using-the-api/" %}
[using-the-api](using-the-api/)
{% endcontent-ref %}
