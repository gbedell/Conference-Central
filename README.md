App Engine application for the Udacity training course.

## Products
- [App Engine][1]

## Language
- [Python][2]

## APIs
- [Google Cloud Endpoints][3]

## Setup Instructions
1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
1. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console][4].
1. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
1. (Optional) Mark the configuration files as unchanged as follows:
   `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
1. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080][5].)
1. (Optional) Generate your client library(ies) with [the endpoints tool][6].
1. Deploy your application.


[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool


## Task 1: Design Choices

Individual conference sessions and their corresponding speakers are created using the createSession endpoint. To create a session, simply nagivate to the createSession endpoint from the
API Explorer. Next, in the Request body box, click and enter values for each field. For the websafeKey, enter the websafeKey of the conference you wish to create a session for. This can be found in the URL for that specific conference when viewing the conference page on the application.

When entering the date, be sure to format like YYYY-MM-DD. This makes it possible to sort the sessions by date. For the startTime, be sure to format like HH:MM, also so that it makes it possibly to sort by time. Lastly, be sure to enter the duration in minutes.


## Task 3: Query Problem

The two additional queries I implemented are getSessionsBySpeaker and getSessionsSortedByDuration. The first is useful if you are interested in what sessions a specific speaker is speaking in. If they are speaking in more than one, someone may want to see all of their speaking sessions. getSessionsSortedByDuration returns a list of all sessions sorted by their duration, descending. This is helpful to see the longest sessions in the application.


The problem with the provided query is that it's an inequality query that has two unique properties. The Datastore only supports inequality queries with one property. 

The first property is the startTime before 7pm, and the second property is NOT being a workshop. Because of Datastore limitations, both of these properties cannot be used in the same query.

One way to work around this limitation is two use two separate queries. For example, in the first query, you could get all sessions that are not workshops. From the first query, you could then query each non-workshop session to see if the startTime is before 7pm. The result would be all non-workshop sessions before 7pm.
