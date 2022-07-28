---
description: Methods for downloading data from the Materials Project (MP) database.
---

# How do I download the Materials Project database?

The Materials Project is an open resource, with data freely available for all. This includes making its data as easy to download as possible including in bulk. Typically, downloading data manually can be slow and tedious, so we provide an API [(Application Programming Interface)](https://en.wikipedia.org/wiki/API) so that data can be downloaded using popular programming languages.

The Materials Project API defines a standardized manner in which the Materials Project database can be accessed by its users. The API is typically used by scientific researchers who need to retrieve large amounts of data to support their research.

For technical users, this is a REST API, meaning that it uses standard HTTP methods for communication, while conforming to the while conforming to the [representational state transfer](https://en.wikipedia.org/wiki/Representational\_state\_transfer) architecture style of software.

There are several APIs offered by the Materials Project:

* **The main API**, giving access to all data primarily generated or maintained by the Materials Project. This API has been in active development over 2020–2022 and is now ready for use by external users.
* **The legacy API**, which gives access to the same category of data, and was developed over the period 2011–2020. This API is still the one used by most users but will not see the addition of new data or features.
* **The MPContribs API**, which gives access to user-contributed data that is also accessible on the Materials Project.

{% content-ref url="differences-between-new-and-legacy-api.md" %}
[differences-between-new-and-legacy-api.md](differences-between-new-and-legacy-api.md)
{% endcontent-ref %}

To use the API, you can use any client that can make HTTP requests (e.g. GET, POST). However, we recommend and support an official Python client. **This is the best way to interact with the Materials Project API.** Using the official Python client allows full integration with Materials Project software like _pymatgen_.

{% content-ref url="using-the-api/" %}
[using-the-api](using-the-api/)
{% endcontent-ref %}
