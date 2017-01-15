# Chapter 2 - Core Defense Mechanisms

1. Why are an application's mechanisms for handling user access only as strong as the weakest of
these components?

Normally there is only one failure mode for preventing unauthorized user access: allowing
the user to authenticate fraudulently. Any of the security mechanisms failing could allow for this
to occur, so the weakest one results in the same issues as the strongest. The whole chain breaks
if one link does.

2. What is the difference between a session and a session token?

The session is the state of the application with respect to a specific user, e.g. the items in
their cart, the last page they were on, whether they are logged in, etc. The session token is
a (usually) series of numbers and letters that acts as a token to map to this state in the
server application.

3. Why is it not always possible to use a whitelist-based approach to input validation?

Sometimes the users will want to legitimately post data that will appear malicious, such as
<script> tags or SQL statements. This is especially common in comment sections of security blogs.
By preventing this, the application is not fulfilling its expected purpose.

4. You are attacking an application that implements an administrative function. You do not have any
valid credentials to use the function. Why should you nevertheless pay close attention to it?

There may be ways to access the function that bypass the administrative login panel or allow the use
of a default set of credentials in special cases. If these can be discovered, access can be granted
to the function without credentials and so is worth the time to check.

5. An input validation mechanism designed to block cross-site scripting attacks performs the
following sequence of steps on an item of input:
  1. Strip any <script> expressions that appear.
  2. Truncate the input to 50 characters.
  3. Remove any quotation marks within the input.
  4. URL-decode the input.
  5. If any items were deleted, return to step 1.
 
Can you bypass this validation mechanism to smuggle the following data past it?
`"><script>alert("foo")</script>`

```
%22>%3Cscript>alert(%22foo%22)%3C/script>
```

1. Nothing is found for a `<script>` tag. (Note: I assumed closing script tags would be deleted.
  according to the author's answers, this is not true.)
2. The input comes in at 42 characters. Success!
3. Quotation marks are encoded. Nothing is found.
4. Decoding the quotes and left angle brackets returns it to valid HTML.
5. Nothing technically was deleted, so it's determined as valid.
