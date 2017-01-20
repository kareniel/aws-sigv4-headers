# v1.2.0

- add support for multiple header parameters through params['x-amz']
- add support for payloads (for post and put) through params['payloadHash']

# v1.1.0

- add support for single query in query string. please note that multiple queries might not work; the the parameter names need to be sorted by character code point in ascending order. 