Glip Webhook Client in Go
=========================

[![Go Report Card][goreport-svg]][goreport-link]
[![Docs][docs-godoc-svg]][docs-godoc-link]
[![License][license-svg]][license-link]
[![Chat][chat-svg]][chat-link]

## Installation

```bash
$ go get github.com/grokify/glip-go-webhook
```

## Usage

1. Create a webhook URL for a conversation in Glip
2. Use the code below to send a message to the webhook URL

```go
import (
    "fmt"
    "github.com/grokify/glip-go-webhook"
)

func sendMessage() {
    // Can instantiate webhook client with full URL or GUID only
    url := "https://hooks.glip.com/webhook/00001111-2222-3333-4444-555566667777"
    client, err := glipwebhook.NewGlipWebhookClient(url)
    if err != nil {
        panic("BAD URL")
    }

    msg := glipwebhook.GlipWebhookMessage{
        Icon:     "https://raw.githubusercontent.com/grokify/glip-go-webhook/master/glip_gopher_600x600xfff.png",
        Activity: "Gopher [Bot]",
        Title:    "Test Message Title",
        Body:     "Test Message Body"}

    resp, err := client.PostMessage(msg)

    respBodyBytes, err := client.SendMessage(msg)
    if err == nil {
        fmt.Printf("%v\n", string(respBodyBytes))
    }
}
```

### Using `fasthttp` client

Posts can be made using [`fasthttp`](https://github.com/valyala/fasthttp).

```go
import (
    "fmt"
    "github.com/grokify/glip-go-webhook"
)

func sendMessage() {
    // Can instantiate webhook client with full URL or GUID only
    url := "https://hooks.glip.com/webhook/00001111-2222-3333-4444-555566667777"
    client, err := glipwebhook.NewGlipWebhookClientFast(url)
    if err != nil {
        panic("BAD URL")
    }

    msg := glipwebhook.GlipWebhookMessage{
        Body: "Test Message Body"}

    res, resp, err := client.PostMessageFast(msg)
    if err == nil {
        fmt.Println(string(resp.Body()))
    }
    fasthttp.ReleaseRequest(req)
    fasthttp.ReleaseResponse(resp)
}
```

You can reuse the client for different Webhook URLs or GUIDs as follows:

```go
// Webhook URL
res, resp, err := client.PostWebhookFast(url, msg)

// Webhook GUID
res, resp, err := client.PostWebhookGUIDFast(guid, msg)
```

 [goreport-svg]: https://goreportcard.com/badge/github.com/grokify/glip-go-webhook
 [goreport-link]: https://goreportcard.com/report/github.com/grokify/glip-go-webhook
 [docs-godoc-svg]: https://img.shields.io/badge/docs-godoc-blue.svg
 [docs-godoc-link]: https://godoc.org/github.com/grokify/glip-go-webhook
 [license-svg]: https://img.shields.io/badge/license-MIT-blue.svg
 [license-link]: https://github.com/grokify/glip-go-webhook/blob/master/LICENSE.md
 [chat-svg]: https://img.shields.io/badge/chat-on glip-orange.svg
 [chat-link]: https://glipped.herokuapp.com/
