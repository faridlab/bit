# Protocol Buffers (Protobuf) Cheatsheet

## Table of Contents
1. [Introduction](#introduction)
2. [Basic Syntax](#basic-syntax)
3. [Data Types](#data-types)
4. [Message Definitions](#message-definitions)
5. [Service Definitions](#service-definitions)
6. [Advanced Features](#advanced-features)
7. [Code Generation](#code-generation)
8. [Use Cases & Examples](#use-cases--examples)
9. [Best Practices](#best-practices)
10. [Common Patterns](#common-patterns)

## Introduction

Protocol Buffers (protobuf) is a language-neutral, platform-neutral, extensible mechanism for serializing structured data. It's widely used for communication protocols, data storage, and more.

### Key Benefits
- **Efficient**: Smaller size than XML/JSON
- **Fast**: Faster serialization/deserialization
- **Cross-platform**: Works across languages
- **Backward/Forward compatible**: Schema evolution
- **Type-safe**: Strong typing system

## Basic Syntax

### File Structure
```protobuf
syntax = "proto3";

package com.example;

import "google/protobuf/timestamp.proto";

// Message definitions
// Service definitions
```

### Comments
```protobuf
// Single line comment
/* Multi-line
   comment */
```

## Data Types

### Scalar Types

| Protobuf Type | Default Value | C++ | Java | Python | Go |
|---------------|---------------|-----|------|--------|-----|
| `double` | 0 | double | double | float | float64 |
| `float` | 0 | float | float | float | float32 |
| `int32` | 0 | int32 | int | int | int32 |
| `int64` | 0 | int64 | long | int/long | int64 |
| `uint32` | 0 | uint32 | int | int | uint32 |
| `uint64` | 0 | uint64 | long | int/long | uint64 |
| `sint32` | 0 | int32 | int | int | int32 |
| `sint64` | 0 | int64 | long | int/long | int64 |
| `fixed32` | 0 | uint32 | int | int | uint32 |
| `fixed64` | 0 | uint64 | long | int/long | uint64 |
| `sfixed32` | 0 | int32 | int | int | int32 |
| `sfixed64` | 0 | int64 | long | int/long | int64 |
| `bool` | false | bool | boolean | bool | bool |
| `string` | "" | string | String | str/unicode | string |
| `bytes` | empty | string | ByteString | bytes | []byte |

### Well-Known Types
```protobuf
import "google/protobuf/any.proto";
import "google/protobuf/timestamp.proto";
import "google/protobuf/duration.proto";
import "google/protobuf/struct.proto";
import "google/protobuf/wrappers.proto";

message Example {
  google.protobuf.Timestamp created_at = 1;
  google.protobuf.Duration duration = 2;
  google.protobuf.Any data = 3;
  google.protobuf.Struct metadata = 4;
  google.protobuf.StringValue optional_string = 5;
}
```

## Message Definitions

### Basic Message
```protobuf
syntax = "proto3";

message Person {
  string name = 1;
  int32 age = 2;
  string email = 3;
}
```

### Nested Messages
```protobuf
message Address {
  string street = 1;
  string city = 2;
  string country = 3;
  int32 zip_code = 4;
}

message Person {
  string name = 1;
  int32 age = 2;
  Address address = 3;
}
```

### Enums
```protobuf
enum Status {
  UNKNOWN = 0;
  ACTIVE = 1;
  INACTIVE = 2;
  PENDING = 3;
}

message User {
  string id = 1;
  string name = 2;
  Status status = 3;
}
```

### Repeated Fields (Arrays)
```protobuf
message Company {
  string name = 1;
  repeated string employees = 2;
  repeated Address offices = 3;
}
```

### Maps
```protobuf
message UserPreferences {
  map<string, string> settings = 1;
  map<int32, string> categories = 2;
}
```

### Oneof Fields
```protobuf
message Notification {
  string user_id = 1;
  oneof notification_type {
    string email = 2;
    string sms = 3;
    string push = 4;
  }
}
```

### Optional Fields (proto3)
```protobuf
message User {
  string name = 1;
  optional string email = 2;  // proto3 optional
  string phone = 3;
}
```

## Service Definitions

### Basic Service
```protobuf
service UserService {
  rpc GetUser(GetUserRequest) returns (GetUserResponse);
  rpc CreateUser(CreateUserRequest) returns (CreateUserResponse);
  rpc UpdateUser(UpdateUserRequest) returns (UpdateUserResponse);
  rpc DeleteUser(DeleteUserRequest) returns (DeleteUserResponse);
}

message GetUserRequest {
  string user_id = 1;
}

message GetUserResponse {
  User user = 1;
}

message CreateUserRequest {
  string name = 1;
  string email = 2;
}

message CreateUserResponse {
  string user_id = 1;
  bool success = 2;
}
```

### Streaming Services
```protobuf
service ChatService {
  // Unary RPC
  rpc SendMessage(Message) returns (MessageResponse);

  // Server streaming
  rpc GetMessages(GetMessagesRequest) returns (stream Message);

  // Client streaming
  rpc UploadMessages(stream Message) returns (UploadResponse);

  // Bidirectional streaming
  rpc Chat(stream ChatMessage) returns (stream ChatMessage);
}
```

### Advanced Service Example
```protobuf
service ECommerceService {
  // Product management
  rpc CreateProduct(CreateProductRequest) returns (Product);
  rpc GetProduct(GetProductRequest) returns (Product);
  rpc ListProducts(ListProductsRequest) returns (stream Product);
  rpc UpdateProduct(UpdateProductRequest) returns (Product);
  rpc DeleteProduct(DeleteProductRequest) returns (google.protobuf.Empty);

  // Order management
  rpc CreateOrder(CreateOrderRequest) returns (Order);
  rpc GetOrder(GetOrderRequest) returns (Order);
  rpc ListOrders(ListOrdersRequest) returns (stream Order);
  rpc UpdateOrderStatus(UpdateOrderStatusRequest) returns (Order);

  // Inventory
  rpc CheckInventory(CheckInventoryRequest) returns (InventoryResponse);
  rpc UpdateInventory(UpdateInventoryRequest) returns (InventoryResponse);

  // Analytics
  rpc GetSalesReport(GetSalesReportRequest) returns (SalesReport);
  rpc GetProductAnalytics(GetProductAnalyticsRequest) returns (stream AnalyticsData);
}

message Product {
  string id = 1;
  string name = 2;
  string description = 3;
  double price = 4;
  int32 stock = 5;
  repeated string categories = 6;
  google.protobuf.Timestamp created_at = 7;
  google.protobuf.Timestamp updated_at = 8;
}

message Order {
  string id = 1;
  string user_id = 2;
  repeated OrderItem items = 3;
  double total_amount = 4;
  OrderStatus status = 5;
  Address shipping_address = 6;
  google.protobuf.Timestamp created_at = 7;
}

message OrderItem {
  string product_id = 1;
  int32 quantity = 2;
  double unit_price = 3;
}

enum OrderStatus {
  PENDING = 0;
  CONFIRMED = 1;
  SHIPPED = 2;
  DELIVERED = 3;
  CANCELLED = 4;
}
```

## Advanced Features

### Field Options
```protobuf
message User {
  string name = 1 [deprecated = true];
  string email = 2 [(google.api.field_behavior) = REQUIRED];
  string phone = 3 [(validate.rules).string = {min_len: 10, max_len: 15}];
}
```

### Custom Options
```protobuf
import "google/protobuf/descriptor.proto";

extend google.protobuf.FieldOptions {
  string custom_option = 50001;
}

message Example {
  string field = 1 [(custom_option) = "value"];
}
```

### Reserved Fields
```protobuf
message Example {
  reserved 2, 15, 9 to 11;
  reserved "foo", "bar";
  string name = 1;
  // string old_field = 2;  // This would cause error
}
```

### Import and Package
```protobuf
syntax = "proto3";

package com.company.ecommerce.v1;

import "google/protobuf/timestamp.proto";
import "common/user.proto";

option go_package = "github.com/company/ecommerce/pb";
option java_package = "com.company.ecommerce.pb";
option java_outer_classname = "ECommerceProto";
```

## Code Generation

### Compiler Installation
```bash
# Install protoc
# macOS
brew install protobuf

# Ubuntu/Debian
apt-get install protobuf-compiler

# Or download from GitHub releases
```

### Generate Code
```bash
# Generate for multiple languages
protoc --go_out=. --go_opt=paths=source_relative \
       --go-grpc_out=. --go-grpc_opt=paths=source_relative \
       --java_out=. \
       --python_out=. \
       --js_out=import_style=commonjs:. \
       user.proto

# Generate with plugins
protoc --plugin=protoc-gen-grpc-java \
       --grpc-java_out=. \
       --java_out=. \
       service.proto
```

### Language-Specific Examples

#### Go
```go
// Generated code usage
import pb "your-project/pb"

func main() {
    user := &pb.User{
        Name:  "John Doe",
        Age:   30,
        Email: "john@example.com",
    }

    data, err := proto.Marshal(user)
    if err != nil {
        log.Fatal(err)
    }

    var newUser pb.User
    err = proto.Unmarshal(data, &newUser)
    if err != nil {
        log.Fatal(err)
    }
}
```

#### Python
```python
# Generated code usage
import user_pb2

def main():
    user = user_pb2.User()
    user.name = "John Doe"
    user.age = 30
    user.email = "john@example.com"

    # Serialize
    data = user.SerializeToString()

    # Deserialize
    new_user = user_pb2.User()
    new_user.ParseFromString(data)
```

#### JavaScript/Node.js
```javascript
// Generated code usage
const { User } = require('./user_pb');

function main() {
    const user = new User();
    user.setName('John Doe');
    user.setAge(30);
    user.setEmail('john@example.com');

    // Serialize
    const data = user.serializeBinary();

    // Deserialize
    const newUser = User.deserializeBinary(data);
}
```

## Use Cases & Examples

### 1. Microservices Communication
```protobuf
// user-service.proto
syntax = "proto3";

package user.v1;

service UserService {
  rpc GetUser(GetUserRequest) returns (User);
  rpc CreateUser(CreateUserRequest) returns (User);
  rpc UpdateUser(UpdateUserRequest) returns (User);
  rpc DeleteUser(DeleteUserRequest) returns (google.protobuf.Empty);
}

message User {
  string id = 1;
  string name = 2;
  string email = 3;
  google.protobuf.Timestamp created_at = 4;
}

message GetUserRequest {
  string user_id = 1;
}

message CreateUserRequest {
  string name = 1;
  string email = 2;
}
```

### 2. Data Storage and Serialization
```protobuf
// analytics.proto
syntax = "proto3";

package analytics.v1;

message Event {
  string event_id = 1;
  string user_id = 2;
  string event_type = 3;
  map<string, string> properties = 4;
  google.protobuf.Timestamp timestamp = 5;
}

message BatchEvents {
  repeated Event events = 1;
  string batch_id = 2;
  google.protobuf.Timestamp created_at = 3;
}
```

### 3. API Gateway Configuration
```protobuf
// api.proto
syntax = "proto3";

package api.v1;

message APIRequest {
  string method = 1;
  string path = 2;
  map<string, string> headers = 3;
  bytes body = 4;
}

message APIResponse {
  int32 status_code = 1;
  map<string, string> headers = 2;
  bytes body = 3;
}

service APIGateway {
  rpc ProcessRequest(APIRequest) returns (APIResponse);
}
```

### 4. Real-time Chat System
```protobuf
// chat.proto
syntax = "proto3";

package chat.v1;

service ChatService {
  rpc JoinChat(JoinChatRequest) returns (stream ChatMessage);
  rpc SendMessage(ChatMessage) returns (ChatResponse);
  rpc LeaveChat(LeaveChatRequest) returns (google.protobuf.Empty);
}

message ChatMessage {
  string id = 1;
  string user_id = 2;
  string room_id = 3;
  string content = 4;
  MessageType type = 5;
  google.protobuf.Timestamp timestamp = 6;
}

enum MessageType {
  TEXT = 0;
  IMAGE = 1;
  FILE = 2;
  SYSTEM = 3;
}

message JoinChatRequest {
  string user_id = 1;
  string room_id = 2;
}

message LeaveChatRequest {
  string user_id = 1;
  string room_id = 2;
}
```

### 5. IoT Data Collection
```protobuf
// iot.proto
syntax = "proto3";

package iot.v1;

message SensorData {
  string device_id = 1;
  string sensor_type = 2;
  double value = 3;
  string unit = 4;
  google.protobuf.Timestamp timestamp = 5;
  map<string, string> metadata = 6;
}

message DeviceStatus {
  string device_id = 1;
  bool online = 2;
  double battery_level = 3;
  string firmware_version = 4;
  google.protobuf.Timestamp last_seen = 5;
}

service IoTService {
  rpc StreamSensorData(stream SensorData) returns (DataResponse);
  rpc GetDeviceStatus(GetDeviceStatusRequest) returns (DeviceStatus);
  rpc UpdateDeviceConfig(UpdateDeviceConfigRequest) returns (ConfigResponse);
}
```

## Best Practices

### 1. Naming Conventions
```protobuf
// Use PascalCase for messages and enums
message UserProfile {
  string user_id = 1;
  string full_name = 2;
}

enum OrderStatus {
  PENDING = 0;
  CONFIRMED = 1;
}

// Use snake_case for fields
message User {
  string first_name = 1;
  string last_name = 2;
  string email_address = 3;
}
```

### 2. Field Numbers
```protobuf
message User {
  string id = 1;           // Reserved for primary key
  string name = 2;         // Basic fields: 1-15
  string email = 3;
  int32 age = 4;

  // Optional fields: 16+
  optional string phone = 16;
  optional Address address = 17;
}
```

### 3. Versioning Strategy
```protobuf
// v1/user.proto
syntax = "proto3";
package user.v1;

message User {
  string id = 1;
  string name = 2;
  string email = 3;
}

// v2/user.proto
syntax = "proto3";
package user.v2;

message User {
  string id = 1;
  string name = 2;
  string email = 3;
  string phone = 4;        // New field
  // string old_field = 5; // Removed field
}
```

### 4. Error Handling
```protobuf
message APIResponse {
  bool success = 1;
  string message = 2;
  int32 error_code = 3;
  oneof result {
    User user = 4;
    ErrorDetails error = 5;
  }
}

message ErrorDetails {
  string code = 1;
  string description = 2;
  map<string, string> details = 3;
}
```

## Common Patterns

### 1. Pagination
```protobuf
message ListUsersRequest {
  int32 page = 1;
  int32 page_size = 2;
  string filter = 3;
  string sort_by = 4;
}

message ListUsersResponse {
  repeated User users = 1;
  int32 total_count = 2;
  int32 page = 3;
  int32 page_size = 4;
  bool has_next = 5;
}
```

### 2. Search and Filtering
```protobuf
message SearchRequest {
  string query = 1;
  repeated string categories = 2;
  double min_price = 3;
  double max_price = 4;
  google.protobuf.Timestamp created_after = 5;
  google.protobuf.Timestamp created_before = 6;
  int32 limit = 7;
  int32 offset = 8;
}
```

### 3. Bulk Operations
```protobuf
message BulkCreateUsersRequest {
  repeated CreateUserRequest users = 1;
  bool skip_errors = 2;
}

message BulkCreateUsersResponse {
  repeated User created_users = 1;
  repeated string errors = 2;
  int32 success_count = 3;
  int32 error_count = 4;
}
```

### 4. Event Sourcing
```protobuf
message Event {
  string event_id = 1;
  string aggregate_id = 2;
  string event_type = 3;
  int64 version = 4;
  google.protobuf.Any data = 5;
  google.protobuf.Timestamp timestamp = 6;
  string correlation_id = 7;
}

message EventStore {
  repeated Event events = 1;
  string aggregate_id = 2;
  int64 current_version = 3;
}
```

## Performance Tips

### 1. Field Number Optimization
```protobuf
// Use 1-15 for frequently used fields (1 byte encoding)
message OptimizedUser {
  string id = 1;        // Most frequently accessed
  string name = 2;      // Frequently accessed
  string email = 3;     // Frequently accessed
  string phone = 4;     // Less frequent
  string address = 5;   // Less frequent
}
```

### 2. Message Size Optimization
```protobuf
// Use appropriate numeric types
message OptimizedData {
  int32 small_number = 1;    // Instead of int64 for small numbers
  float precision_value = 2; // Instead of double if precision allows
  bool flag = 3;             // Use bool instead of int32 for flags
}
```

### 3. Streaming for Large Data
```protobuf
service DataService {
  // For large datasets, use streaming
  rpc GetLargeDataset(GetDatasetRequest) returns (stream DataChunk);
  rpc UploadLargeDataset(stream DataChunk) returns (UploadResponse);
}

message DataChunk {
  int32 chunk_number = 1;
  bytes data = 2;
  bool is_last = 3;
}
```

## Integration Examples

### gRPC with Go
```go
// server.go
package main

import (
    "context"
    "log"
    "net"

    pb "your-project/pb"
    "google.golang.org/grpc"
)

type userServer struct {
    pb.UnimplementedUserServiceServer
}

func (s *userServer) GetUser(ctx context.Context, req *pb.GetUserRequest) (*pb.User, error) {
    // Implementation
    return &pb.User{
        Id:   req.UserId,
        Name: "John Doe",
        Email: "john@example.com",
    }, nil
}

func main() {
    lis, err := net.Listen("tcp", ":50051")
    if err != nil {
        log.Fatal(err)
    }

    s := grpc.NewServer()
    pb.RegisterUserServiceServer(s, &userServer{})

    log.Println("Server starting on :50051")
    if err := s.Serve(lis); err != nil {
        log.Fatal(err)
    }
}
```

### gRPC with Python
```python
# server.py
import grpc
from concurrent import futures
import user_pb2
import user_pb2_grpc

class UserService(user_pb2_grpc.UserServiceServicer):
    def GetUser(self, request, context):
        return user_pb2.User(
            id=request.user_id,
            name="John Doe",
            email="john@example.com"
        )

def serve():
    server = grpc.server(futures.ThreadPoolExecutor(max_workers=10))
    user_pb2_grpc.add_UserServiceServicer_to_server(UserService(), server)
    server.add_insecure_port('[::]:50051')
    server.start()
    server.wait_for_termination()

if __name__ == '__main__':
    serve()
```

## Tools and Libraries

### Command Line Tools
```bash
# protoc - Protocol Buffer compiler
protoc --version

# buf - Modern protobuf tooling
buf --version
buf lint
buf generate
buf breaking --against '.git#branch=main'

# grpcurl - Command line gRPC client
grpcurl -plaintext localhost:50051 list
grpcurl -plaintext localhost:50051 user.UserService/GetUser -d '{"user_id": "123"}'
```

### Popular Libraries
- **Go**: `google.golang.org/protobuf`, `google.golang.org/grpc`
- **Python**: `grpcio`, `grpcio-tools`, `protobuf`
- **Java**: `io.grpc:grpc-*`, `com.google.protobuf:protobuf-java`
- **JavaScript/Node.js**: `@grpc/grpc-js`, `@grpc/proto-loader`
- **C++**: `grpc++`, `protobuf`

This comprehensive protobuf cheatsheet covers everything from basic syntax to advanced patterns and real-world examples. Use it as a reference for your protobuf development needs!
