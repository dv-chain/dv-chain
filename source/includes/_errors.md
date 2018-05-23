# Errors

<!-- <aside class="notice">This error section is stored in a separate file in `includes/_errors.md`. Whiteboard allows you to optionally separate out your docs into many files...just save them to the `includes` folder and add them to the top of your `index.md`'s frontmatter. Files are included in the order listed.</aside> -->

The API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- There is something wrong with the request
401 | Unauthorized -- Your API key is wrong
403 | Forbidden -- The endpoint requested is hidden for administrators only
404 | Not Found -- The specified kitten could not be found
406 | Not Acceptable -- You requested a format that isn't json
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarially offline for maintanance. Please try again later.
