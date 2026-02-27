# RS485 Signal Path

Signal trace from the FM33LC026N MCU to the RS485 bus, through isolators and transceiver.

```mermaid
flowchart LR
    MCU["FM33LC026N\nMCU"]

    subgraph UART1["UART1 (pins 31 & 32)"]
        direction TB
        TX1["TX"]
        RX1["RX"]
    end

    subgraph ISO1["CA-IS3721HS\nIsolator #1"]
        direction TB
        VI1_1["VI1"]
        VO2_1["VO2"]
        VO1_1["VO1"]
        VI2_1["VI2"]
    end

    subgraph TP["3PEAK TP8485E\nRS485 Transceiver"]
        direction TB
        DI["DI"]
        RO["RO"]
        RE["RE"]
        DE["DE"]
        A["A"]
        B["B"]
    end

    subgraph BUS["RS485 Bus"]
        direction TB
        RS_A["A"]
        RS_B["B"]
    end

    MCU --- UART1
    TX1 --> VI1_1
    VO2_1 --> RX1
    VO1_1 --> DI
    RO --> VI2_1
    A --> RS_A
    B --> RS_B

    subgraph UART4["UART4 (pins 5 & 6)"]
        direction TB
        TX4["TX"]
        RX4["RX"]
    end

    subgraph ISO2["CA-IS3721HS\nIsolator #2"]
        direction TB
        VI1_2["VI1"]
        VO2_2["VO2"]
        VO1_2["VO1"]
        VI2_2["VI2"]
    end

    MCU --- UART4
    TX4 --> VI1_2
    VO2_2 --> RX4
```

> **Note:** The original Excalidraw source is preserved in
> [`rs485-trace-mapping.excalidraw.md`](rs485-trace-mapping.excalidraw.md) for reference.
