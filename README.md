# Facebook Login for Web with RapidAPI


#### Steps for Using Facebook Login With the JavaScript SDK

There are a few steps that you will need to follow to integrate Facebook Login into your web application, most of which are included in the quickstart example at the top of this page. 

###### At a high level those are:
Checking the login status to see if someone's already logged into your app. During this step, you also should check to see if someone has previously logged into your app, but is not currently logged in.
If they are not logged in, invoke the login dialog and ask for a set of data permissions.

* Verify their identity.
* Store the resulting access token.
* Make API calls.
* Log out.

[This is the full Facebook SDK documentation for the javascript SDK.](https://developers.facebook.com/docs/facebook-login/web)

### How can we work with RapidAPI?

So after we have understood the basics of authnticating with Facebook through Javascript SDK, now we need to connect it to a backend service that can add more logic to our application.

###### Fast forward

Basiclly, each time that user accepted our application with his account we have to save his token to create an actions with his profile.

First, let's add a new endpoint through RapidAPI's dashboard:

![Adding endpoint to dashboard](http://g.recordit.co/SIYiIu5yy5.gif)

Now, after we have created the endpoint and opened the editor for this endpoint, we need to drop in the Database.save block:
![Adding an database block to endpoint](http://g.recordit.co/p5ryhpRosT.gif)

Mazal Tov! You are a proud owner of an endpoint! The endpoint now saves data to the database with user's token.
Now let's generate a code that calls to our endpoint from remote app. For this one we are using a regular JQuery Ajax function.

Here is the proccess of how we can get it:
![Getting JQuery code](http://g.recordit.co/zMFwBm6f7e.gif)


So this is the code that we got from the generator:

```javascript
jQuery.ajax({
    url: "http://rapidapi-ebay-testing.rapidapi.io/savetoken",
    type: "POST",
    contentType: "application/x-www-form-urlencoded",
    data: {
        "fullname": "example",
        "fbId": "example",
        "token": "example",
    },
})
.done(function(data, textStatus, jqXHR) {
    console.log("HTTP Request Succeeded: " + jqXHR.status);
    console.log(data);
})
.fail(function(jqXHR, textStatus, errorThrown) {
    console.log("HTTP Request Failed");
})
.always(function() {
    /* ... */
});
```

Now, when we have the code for integration with RapidAPI, we just need to put it inside the code of the authntication callback in the index.html file.

```javascript
function sendAuthToRapidAPI() {
		console.log('Welcome!  Fetching your information.... ');
		FB.api('/me', function(response) {
			jQuery.ajax({
			    url: "http://rapidapi-ebay-testing.rapidapi.io/savetoken",
			    type: "POST",
			    contentType: "application/x-www-form-urlencoded",
			    data: {
			        "fullname": response.name,
			        "fbId": response.id,
			        "token": response.token,
			    },
			})
			.done(function(data, textStatus, jqXHR) {
			    console.log("HTTP Request Succeeded: " + jqXHR.status);
			    console.log(data);
			})
			.fail(function(jqXHR, textStatus, errorThrown) {
			    console.log("HTTP Request Failed");
			})
			.always(function() {
			    /* ... */
			});
		});
	}
```
