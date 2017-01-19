#aws-sigv4-headers

Implements AWS Requests with [Signature Version 4][sigv4]. 
Saves you from the pain of using the AWS SDK.

Keep in mind this has only been tested with API Gateway and S3.


## install

`npm install --save aws-sigv4`

## usage

```js
// .config.js
// make sure you add this file to .gitignore
module.exports {
  "aws_access_key_id": "",
  "aws_secret_access_key": "",
  "region": ""
}
```

*node.js example*

```js
const https = require('https')
const fs = require('fs')
const getHeaders = require('aws-sigv4-headers')
const config = require('./config')
const headers = getHeaders(config, {
  params.service = 'apigateway' // or 's3'
  params.canonicalURI = `/restapis/${restApiId}/stages/${stage}/exports/swagger`
})

const options = {
  method: 'get',
  path: params.canonicalURI,
  host: headers.host,
  headers: headers
}

const fetchSwaggerExport = (options, callback) => {
  https.get(options, res => {
    let out = ''

    res.on('data', data => {
      data = data.toString()
      if (res.statusCode === 403) return callback(JSON.parse(data))
      out += data
    })

    res.on('end', () => callback(null, JSON.parse(out)))
  }).on('error', err => callback(err))
}

fetchSwaggerExport(options, (err, swag) => {
  if (err) return console.error(err)
  console.log(swag) // -> { "swagger": "2.0", ... }
})

```

*browser*

for the browser, you can use the [xhr][xhr] library instead of the node https module.


[sigv4]: http://docs.aws.amazon.com/general/latest/gr/sigv4_signing.html
[xhr]: https://github.com/naugtur/xhr
