# CH3: Requests

## L1: TCP to HTTP
<p>
    The tcp guarantee the data is in order and receive all the data, but not tell you the type of data.
</p>

<p>
    The HTTP tells you the type of data
</p>

### HTTP-message (Format)
<p>
    start-line CRLF
    ( field-line CRLF )
    CRLF
    [ message-body ]
</p>

CRLF is (\r\n) a carriage return followed by a line feed.
Example
| Part | Example | Description |
|------|---------|-------------|
| `start-line CRLF` | `POST /users/primeagen HTTP/1.1` | The request (for a request) or status (for a response) line |
| `*( field-line CRLF )` | `Host: google.com` | Zero or more lines of HTTP headers. These are key-value pairs. |
| `CRLF` | | A blank line that separates the headers from the body. |
| `[ message-body ]`     | `{"name": "TheHTTPagen"}`        | The body of the message. This is optional. |

## L3: HTTP Post
<p> 
    curl is a command line tool for making HTTP requests
</p>
