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

## Project task explanations
### Task1: Design choices
#### Session:

Session object has the following items: name, highlights, speaker, duration, typeOfSession, date, startTime.

All of them are StringProperties except for date (DateProperty) and startTime (TimeProperty)

Required fields: name, speaker, typeOfSession

typeOfSession values are predefined in the enum TypeOfSession

####Speaker:

Speaker has been defined as a String to avoid complexity in the data model.

### Task3: Implemented queries:
Query 1: Get all sessions of a given speaker (getSessionsBySpeaker)

Query 2: Get all conferences of a given Topic (getConferencesByTopic)


### Task3: Query related problem 
The problem for implementing the query is that it can't be applied more than one inequality filter for different properties.

My solution for this problem is to workaround the inequality filters with ndb.OR.

Since:

```query = Session.query(Session.typeOfSession != 'workshop')```

is the same as:

```query = Session.query(ndb.OR(Session.typeOfSession < 'workshop', Session.typeOfSession > 'workshop'))```

final result:

```sessions = Session.query(ndb.OR(Session.typeOfSession < 'workshop',
                                    Session.typeOfSession > 'workshop',
                                    ndb.OR(Session.startTime < time(19, 00, 00))))```



[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool

