# ORIGIN Frame Example

## Server

The usage of ORIGIN Frame is dependant on TLS. A simple TLS setup is done by using a self-signed certificate.

Generate a private key for the server as `server.key`

```shell
$ openssl genrsa -out server.key 2048
```

and a self-signed certificate `server.crt` using the key by running:

```shell
$ openssl req -new -x509 -sha256 -key server.key -out server.crt -days 365
```

A minimal `server` example is as follows:

```go
func handleRootWithOriginFrame(writer http.ResponseWriter, request *http.Request) {
	origins := []string{"https://foo.bar", "https://cat.dog"}
	if of, ok := writer.(http.OriginPusher); ok {
		err := of.OriginPush(origins)
		if err != nil {
			fmt.Printf("failed to write origin frame: %v\n", err)
			writer.WriteHeader(500)
			writer.Write([]byte("failed."))
			return
		}
	}
	writer.WriteHeader(200)
	writer.Write([]byte("success."))
}

func main() {
    cert, err := tls.LoadX509KeyPair("server.crt", "server.key")
    if err != nil {
		log.Fatal("failed to load certificate:", err)
    }
    
    s := &http.Server{
        Addr: fmt.Sprintf(":%d", 443),
        TLSConfig: &tls.Config{
            Certificates: []tls.Certificate{cert},
        },
    }

    http.HandleFunc("/", handleRootWithOriginFrame)
    err = s.ListenAndServeTLS("server.crt", "server.key")
    if err != nil {
		panic(fmt.Sprintf("failed to start server %v\n", err))
    }
}
```
