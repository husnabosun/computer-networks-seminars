# Text Protocol Mini-Specification

## 1. Informal Description
This is a simple, text-based request-response protocol designed to manage a remote key-value store over TCP. It uses a "Length-Prefix" framing mechanism to handle the continuous nature of TCP byte streams. The server maintains an internal state (a dictionary) that is protected by thread locks to ensure data integrity during concurrent access.

**Available Commands:**
- `add`: Stores a value (resource) under a specific key.
- `get`: Retrieves the value associated with a key.
- `remove`: Deletes a key-value pair.
- `count`: Returns the total number of keys currently stored.

## 2. Request/Response Format
- **REQUEST** = `<LEN> SP <COMMAND_LINE>`
- **COMMAND_LINE** = `<COMMAND> [SP <KEY> [SP <RESOURCE>...]]` ('[, ]' means optional)
- **RESPONSE** = `<LEN> SP <PAYLOAD>`

*Note: <LEN> is the total character count of the entire message (including the length itself and the space).*

## 3. Command List
- `add <key> <resource...>`: Adds or updates a resource. Requires both key and data.
- `remove <key>`: Deletes the entry for the given key.
- `get <key>`: Returns the stored resource or "key was not found".
- `count`: Returns the current number of keys in the format "<N> keys". Requires no arguments.

## 4. State-Machine Representation
1. **CONNECTED**: The server is waiting for a request.
2. **RECEIVING**: The server reads the length header and collects the full message stream.
3. **PROCESSING**: The server parses the command and interacts with the `State` object.
4. **RESPONDING**: The server frames the result and sends it back to the client.
5. **TRANSITION**: After sending the response, the server returns to the **CONNECTED** state to await the next request.