# CH5: HTTP Headers

## L1: Headers
Each field line consists of a case-insensitive field name followed by a colon (":"), optional leading whitespace, the field line value, and optional trailing whitespace.

<p> field-line = field-name":"OWS field-value OWS</p>
One important point to note: there can be an unlimited amount of whitespace before and after the field-value (Header value). However, when parsing a field-name, there must be no spaces before the field name, nor betwixt the colon and the field name. In other words, these are valid:

'Host: localhost:42069'
'Host:           localhost:42069    '

but this is not:
Host : localhost:42069

## Constraints
We've got our header parsing mostly working, but we're actually not being as strict as we should be to match the RFC. Let's fix that.

### Case Insensitivity
Ever wonder why there's usually a .Get method on a header object to get header values? It's because these darned keys (not necessarily values) are case insensitive! If you use the hash map directly, you'll have to account for Content-Length and content-length being the same on your own.

### Valid Characters
Interestingly, definitions can be spread across multiple RFCs.field-name has an implicit definition of a token as defined by RFC 9110.

In other words, a field-name must contain only:

Uppercase letters: A-Z
Lowercase letters: a-z
Digits: 0-9
Special characters: !, #, $, %, &, ', *, +, -, ., ^, _, `, |, ~

## Multiple Values
We have one more bug in our implementation. To new web developers, this can come as a shock, but it's actually perfectly valid (see RFC 9110 5.2) to have multiple values for a single header key. For example:

Set-Person: lane-loves-go
Set-Person: prime-loves-zig
Set-Person: tj-loves-ocaml

Is perfectly valid. The way we should handle this case is to combine the values into a single string, separated by a comma:

Set-Person: lane-loves-go, prime-loves-zig, tj-loves-ocaml
