---
layout: post_detail
title:  "Token Based Authentication"
brief: "As part of my Props2Me project, I had to think about token based authentication"
image: "posts/props2me-experiments.png"
date:   2015-03-11 23:07:53
tags: angular authentication services props2me-experiments
---

When I originally created Props2Me, I had to think about the authentication scheme I was using.  When I created applications using only Rails, it was easy.  I could use Devise or, fairly quickly, roll my own basic scheme.  It was great because everything lived server-side; my application had access to sessions and the user did not.  However, when the application UI and interaction lives in Angular and only uses the server to get data, things changed.  Now, permissions and roles were accessible to a user and changing a variable could impact the security of the application.

Token based authentication allows roles and permissions to live in the application and be verified by the server.  This way, if a user happens to change a variable, the program will check and compensate server-side.

<div style="text-align: center"><img src="{{site.base_url}}/images/posts/token-based-authentication.jpg" /></div>
<br />

1. Username and password passed to API via login form (POST) for authentication.  
2. If authenticated, server determines whether session totken exists and is not expired (or close to expiring).  If yes, use the existing token from the DB.  If no, generate a random 64-digit number and assign it to the user in the "ApiKey" table along with expiration date.
3. Once authenticated, return user information and session token to the UI
4. Angular stores user information in UserService which is injected into every controller so it can be made available to the UI as something like $scope.currentUser.
5. On each route change, the Angular router checks the UserService for appropriate permissions (guest, user, admin, etc...).  If user permissions match the page permissions, display the page.  Otherwise, redirect user.
6. Each API call in the controller includes session token in the call as URL param.  If the token matches the user ID, is not expired and their role on the User table allows, data will be returned.  Else, the service will return with a 400.
7. When the user logs out, the expiration date is set to yesterday so that it cannot be used again.

In this case, the user could change their role in the UI but because the session token is used to validate the request server side, data for a page that has been accessed incorrectly will never show.

I like this system but I think it could be improved.  In the past, I have sent the session token as a URL param but I think that it would be more secure if placed as a HEAD variable so it isn't as easily exposed when making a call.  Additionally, it might be worth exploring the idea of a public and private key that can be used in combination to validate the user's API requests.