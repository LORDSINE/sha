# SHA256 Class

The SHA256 class contains all the necessary functionality to compute the SHA-256 hash of a given string.

### Constants
- BlockSize: 64 bytes (512 bits), the size of each data block processed by the algorithm.
- HashValues: 8, representing the number of 32-bit words in the final hash output (256 bits).

### Member Variables
- dataBuffer: A buffer to store input data.
- bitLength: Total length of the input data in bits.
- h: An array holding the initial hash values and later the resulting hash.

### Constructor
The constructor initializes the h array with predefined initial hash values, as specified by the SHA-256 standard. It also initializes bitLength to 0.

```
SHA256::SHA256() {
    // Initialize hash values
    h[0] = 0x6a09e667;
    h[1] = 0xbb67ae85;
    h[2] = 0x3c6ef372;
    h[3] = 0xa54ff53a;
    h[4] = 0x510e527f;
    h[5] = 0x9b05688c;
    h[6] = 0x1f83d9ab;
    h[7] = 0x5be0cd19;
    bitLength = 0;
}
```

### update Method
This method updates the hash with a new chunk of data. It appends the new data to the dataBuffer, updates the bitLength, and processes any full 64-byte blocks.

```
void SHA256::update(const std::string& data) {
    dataBuffer.insert(dataBuffer.end(), data.begin(), data.end());
    bitLength += data.size() * 8;
    while (dataBuffer.size() >= BlockSize) {
        processBlock(dataBuffer.data());
        dataBuffer.erase(dataBuffer.begin(), dataBuffer.begin() + BlockSize);
    }
}
```

### processBlock Method
This method processes a 64-byte block of data. It involves:

- Initializing a message schedule w with 64 entries.
- Extending the first 16 words of w into the remaining words using bitwise operations.
- Initializing working variables (a, b, c, d, e, f, g, h_) with the current hash value.
- Performing 64 rounds of the SHA-256 compression function.
- Updating the hash values with the results from the compression rounds.

```
void SHA256::processBlock(const uint8_t block[BlockSize]) {
    // (The function implementation is detailed in the original code)
}
```

### finalize Method
This method pads the message to ensure its length is a multiple of 512 bits and processes any remaining data.

```
void SHA256::finalize() {
    padMessage();
    if (!dataBuffer.empty()) {
        processBlock(dataBuffer.data());
    }
}
```

### padMessage Method
This method pads the message according to the SHA-256 padding rules. It appends a '1' bit, followed by '0' bits, and finally the original message length in bits.

```
void SHA256::padMessage() {
    // (The function implementation is detailed in the original code)
}
```

### digest Method
This method finalizes the hash computation and returns the hash as a hexadecimal string.

```
std::string SHA256::digest() {
    finalize();
    std::ostringstream result;
    for (size_t i = 0; i < HashValues; ++i) {
        result << std::hex << std::setfill('0') << std::setw(8) << h[i];
    }
    return result.str();
}
```

## Helper Functions
These functions perform various bitwise operations required by the SHA-256 algorithm:

- rotr: Right rotation.
- choose: Choice function.
- majority: Majority function.
- sigma0, sigma1: Functions used in the SHA-256 compression function.
- gamma0, gamma1: Functions used to extend the message schedule.

## main Function
The main function demonstrates the use of the SHA256 class. It reads a string from the user, updates the hash with this string, computes the final hash, and prints it.

```
int main() {
    SHA256 sha256;
    std::string input;

    std::cout << "Enter the text to hash: ";
    std::getline(std::cin, input);

    sha256.update(input);
    std::string hash = sha256.digest();

    std::cout << "SHA-256 hash: " << hash << std::endl;
    return 0;
}
```

# Summary
- The SHA256 class implements the SHA-256 hashing algorithm.
- It processes data in 64-byte blocks, performs padding, and computes the hash using bitwise operations.
- The main function demonstrates hashing a user-provided string and printing the result.
