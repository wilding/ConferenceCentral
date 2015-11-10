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



How sessions are implemented:
	The session object is defined in Models.py.  In Models.py, both the protorpc message classes and the datastore kind are defined.  The properties for the Session kind are defined in the class with a specified type, and the protorpc message classes have fields that match the properties.  The properties that are defined in the Session class are: name, highlights, speaker, duration, typeOfSession, date, startTime, websafeConferenceKey, and sessionKey.  Name, highlights, speaker, typeOfSession, and both of the key properties are given the string type.  The name property is required to have a value.  Highlights is specified as a repeated property, meaning it's value is a list of strings rather than a string. The duration property has an integer type that represents the number of minutes the session lasts.  The date and startTime properties have datetime.date and datetime.time types respectively.  It is important to note that the protorpc message class does not support datetime types, so the values are converted between datetime and string when passing datastore property values to protoRPC messages.  I have properties for websafeConferenceKey and sessionKey even though they are redundant because I can't get the code to function otherwise.  Whenever I try to get rid of them, I still get the error "AttributeError: type object 'Session' has no attribute 'sessionKey'/'websafeConferenceKey'" when trying to create a new session.

Two additional query types:
	I wrote two additional endpoints methods.  The first returns the sessions that are under a user-submitted number of minutes.
	This utilizes an inequality filter in the query.  The second returns the sessions that contain a user-submitted highlight.
	Highlights is a repeated property, so this query utilizes a 'member-of' filter.

Problem with provided query:
	The provided query is problematic because it attempts to apply two inequality filters to two different properties.  I
	implemented a solution to this problem by changing the typeOfSession property so that it only accepts a discrete set
	of values.  Then I changed the query so that instead of filtering for sessions where type != workshops, it filters
	for sessions where type == all other possible values besides 'workshop'.

