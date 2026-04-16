# Market Data Benchmarks

This repository compares market data processing implementations across different programming languages, including C#, Java, Python, C++, and Rust.

Each implementation connects to Binance via WebSocket, receives Best Bid Offer (BBO) updates for Bitcoin, calculates the spread, and exposes a simple HTTP endpoint that returns the current spread.

To keep the comparison straightforward and easy to understand, each language implementation is intentionally contained in a single file whenever possible. The goal is to prioritize simplicity, readability, and comparability rather than strict adherence to production architecture, best practices, or maximum optimization for each language.

## Benchmark Goals

The purpose of this project is to compare how similar market data processing programs behave when implemented in different languages.

The benchmark focuses on:

- implementation complexity and development effort
- requests per second handled by the HTTP endpoint
- endpoint latency under load
- CPU and memory usage during stress testing
- deserialization time for messages received from Binance
- logging and processing behavior while handling real-time market data

The implementations are reasonably optimized, but not pushed to their maximum possible performance. The intention is to keep the comparison balanced and practical, without introducing highly specialized optimizations that would make cross-language comparison less fair or harder to understand.

## Stress Testing

Stress testing was performed using **wrk**, a lightweight and efficient HTTP benchmarking tool.

Each test ran for 5 minutes, with progressively higher numbers of threads and connections across multiple runs. The objective was to observe how each implementation behaves under increasing load, with attention to:

- latency
- requests per second
- CPU usage
- memory usage

Other tools were also evaluated.

**k6** was considered, but in this context it consumed more machine resources than the benchmarked applications themselves, which made it less suitable for this comparison.

**JMeter** was also tested, but it proved to be the least efficient option for this type of benchmark.

![Insert image showing the inefficiency of k6 here](./image.png)

## Deserialization and Message Logging Test

In addition to HTTP stress testing, each implementation continuously receives market data from Binance, deserializes the incoming messages, calculates the spread, and writes log entries.

For each received message, the benchmark records:

- the timestamp when the message was received
- the time spent deserializing and processing the message

In this test scenario, all applications run simultaneously, but their HTTP endpoints are not exercised. The focus is exclusively on comparing deserialization, processing, and logging behavior under real-time market data flow.

## Implementations

- C++
- Rust
- Python
- Java
- C#

---

## Running the C++ Implementation

### Requirements

Make sure you have the following tools and libraries installed on your system:

- **CMake** (version 3.10 or higher)
- **C++ compiler** compatible with C++11 or higher
- **Boost**
- **OpenSSL**
- **WebSocket++**

### Installing Dependencies

To install dependencies, you can use the following commands:

```sh
sudo apt-get update
sudo apt-get install cmake g++ libboost-system-dev libssl-dev 
sudo apt-get install libwebsocketpp-dev nlohmann-json3-dev
```

### Building the Project

#### 1. Create a build directory and navigate to it:
```sh
mkdir build
cd build
```

#### 2. Generate build files with CMake:
```sh
cmake -DCMAKE_BUILD_TYPE=Release ..
```

#### 3. Compile the project:
```sh
make
```

#### 4. Running the Program:
```sh
./main <executionTime>
```

## To run Rust

### Requirements

- **Tokio**
- **Tokio-Tungstenite**
- **Serde**
- **Warp**
- Other crates like `chrono`, `simd-json`, and `futures-util` for handling time, data, and asynchronous operations.

#### Compile and running the Program:

```bash
cargo run --release -- <executionTime>
```

## To run python

### Requirements

- **Python 3.8+**
- **pip**

### Installing Dependencies

To install dependencies, you can use the following commands:

```sh
sudo apt install redis
```

```sh
pip install fastapi
pip install websockets
pip install hypercorn
pip install redis
```

#### Compile and running the Program:

```sh
gunicorn --env PARAMS=<executionTime> -w 16 main:app
```

## To run java

### Requirements
- **Java 22** or higher.
- **Maven 3.x**
- **spring-boot 3.x**

#### 1. Compile the project:
```bash
    mvn clean package
```

#### 2. Running the Program:
```bash
java -server -jar target/main-1.0.jar <executionTime>
```

## To run C#

### Requirements
- **Microsoft.AspNetCore.Http**
- **Microsoft.AspNetCore.Routing**
- **Utf8Json**

#### 1. Compile and Running the Program:

```sh
dotnet run --configuration Release -- <executionTime>
```

## Tests


## k6

## wrk

```sh
wrk -t40 -c200 -d30s http://127.0.0.1:8000/Spread
```
