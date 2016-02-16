BasicAuthenticationMiddleware
=============================
[![Swift 2.2](https://img.shields.io/badge/Swift-2.2-orange.svg?style=flat)](https://swift.org)
[![Platform Linux](https://img.shields.io/badge/Platform-Linux-lightgray.svg?style=flat)](https://swift.org)
[![License MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=flat)](https://tldrlegal.com/license/mit-license)
[![Slack Status](https://zewo-slackin.herokuapp.com/badge.svg)](http://slack.zewo.io)

**BasicAuthenticationMiddleware** for **Swift 2.2** provides a fast way to authenticate your endpoints.

## Usage

### Server

```swift
import BasicAuthenticationMiddleware
import HTTPServer
import Router

let basicAuth = BasicAuthenticationMiddleware { username, password in
	if username == "admin" && password == "password" {
		return .Authenticated
	}

	return .AccessDenied
}

let router = Router(middleware: basicAuth) { route in
	route.get("/") { _ in
		return Response(status: .OK, body: "Authenticated")
	}
}

try Server(responder: router).start()
```

If you want to pass forward any custom data in the `Request` storage, you can return a `Payload` with a key and a value.

```swift
let basicAuth = BasicAuthenticationMiddleware { username, password in
	if let user = User.withUsername(username, password: password) {
		return .Payload(key: "user", value: user)
	}

	return .AccessDenied
}
```


### Client

```swift
import BasicAuthenticationMiddleware
import HTTPClient

let basicAuth = BasicAuthenticationMiddleware(
	username: "API_KEY",
	password: "API_SECRET"
)

let client = try Client(host: "your.api.com", port: 80)
let response = try client.get("/", middleware: basicAuth)
```

## Installation

- Add `Basic Auth Middleware` to your `Package.swift`

```swift
import PackageDescription

let package = Package(
	dependencies: [
		.Package(url: "https://github.com/Zewo/BasicAuthenticationMiddleware.git", majorVersion: 0, minor: 2)
	]
)
```

## Community

[![Slack](http://s13.postimg.org/ybwy92ktf/Slack.png)](http://slack.zewo.io)

Join us on [Slack](http://slack.zewo.io).

License
-------

**BasicAuthenticationMiddleware** is released under the MIT license. See LICENSE for details.