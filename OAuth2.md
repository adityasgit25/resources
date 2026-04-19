OAuth2:

So its for the authorization not authentication, just to take the information of yours from another service, without entering password to that app.
Its just gives certain access of your information to that app, you can set the restrictions of your information that you want to provide to other apps.

So what happens is:

1. First the frontend send the user to the backend.
2. The backend redirects it to the google for getting the code.
3. Once the backend gets the code, it request for information from the google for the user, so google returns information about the user.
4. Now based on the information given by the google, your app creates a JWT access token.
5. And there are two types of token, one is access token and other is refresh token.

Access tokens are short-lived and used for the access.
Refresh tokens are long-lived and are used to generate access, and they keep getting rotated(changed) for the security purposes.


Now can you see one security issue in this whole process,
its that anyone can get the code in the point 2 right then and once it receives the code, it can get the information of the user from the google
right, but there's one more thing to it, which is PKCE(Proof Key for Code Exchange).

The information that google is sending is called the Identity token, which has user's information, normally referred as Google Token.

In PKCE what happens is,
1. Our app server generates a random string (code_verifier) and sends a hashed version (code_challenge) along with the initial authorization request.
2. Now app asks for the token(Google token), exchange the code for a token, the app sends the original code_verifier.
3. Now, google hashes the code_verifier and matches it against the initial code_challenge to prove ownership.

And if its good then token is granted otherwise not.

Security Benefit: Even if the authorization code is stolen, it is useless without the original code_verifier, which is never sent over the wire until the final step.


Components:
code_verifier: A high-entropy, random string.
code_challenge: A SHA256 hash of the verifier.
code_challenge_method: Usually "S256" (SHA256) or "plain".


This PKCE thing is handled by the Spring Security itself so you don't need to worry about this thing.

The code_challenge_method is sent to Google's authorization server during the very first step of the login process. 

