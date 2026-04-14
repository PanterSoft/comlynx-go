# comlynx-go

Go client library for reading values from Danfoss TLX inverters over ComLynx (RS485/TCP bridge).

## Requirements

- Go 1.22+
- Access to a ComLynx transport:
  - Local serial device (for example `/dev/ttyUSB0`), or
  - TCP endpoint exposing a raw RS485 bridge

## Install

```bash
go get github.com/evcc-io/comlynx
```

## Run tests

```bash
go test ./...
```

## Quick example

```go
package main

import (
	"log"

	comlynx "github.com/evcc-io/comlynx"
)

func main() {
	client, err := comlynx.New(nil, comlynx.Config{
		Device: "/dev/ttyUSB0",
	})
	if err != nil {
		log.Fatal(err)
	}
	defer client.Close()

	// Set the inverter destination after discovery (example values).
	client.SetDestination(comlynx.Address{Network: 0x0c, Subnet: 0x06, Node: 0xb1})

	value, err := client.Read(comlynx.ParamGridPowerTotal)
	if err != nil {
		log.Fatal(err)
	}

	log.Printf("grid power total: %d", value)
}
```

## Notes

- `Client.Read` returns the raw 32-bit value reported by the inverter.
- `Client.Ping` can be used for address discovery on the ComLynx bus.
