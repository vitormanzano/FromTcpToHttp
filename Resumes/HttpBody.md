# Http Body

A user agent SHOULD send Content-Length in a request...
And "should" has a specific meaning in RFCs per RFC2119:

This word, or the adjective "RECOMMENDED", mean that there may exist valid reasons in particular circumstances to ignore a particular item, but the full implications must be understood and carefully weighed before choosing a different course.

All this to say, for our implementation we're going to assume that if there is no Content-Length header, there is no body present. This is a safe assumption for our purposes, though it might not be true in all cases in the wild.
