### What 
Generate public/private keys using Elliptic Curve Cryptography (ECC)

### Why
Discrete logarithm problem becomes hard when using Elliptic Curves.  
High level steps:  
- Given Elliptic Curve definition: y² = x³+ax+b.  
- Select a random number with 256-bit : n, this is the private key.  
- Take a point G(x, y) on the elliptic curve.  
- Multiply both P(P_x, P_y) = n  * G(x, y). P(P_x, P_y) is the public key  
 


### How (using openssl)
- Select an elliptic curve (NIST recommands a bunch of them). The most popula is "Secp256k1" : y² = x³+7  

- Generate key-pairs:  
  `openssl ecparam -name secp256k1 -genkey -out myKeyPair.pem`  

- myKeyPair.pem (base64 encoded file) contains the private key, the public key and all used information (e.g. secp256k1 ..)  

- Read kay pair information:  
  `openssl ec -in myKeyPair.pem -text -noout`  

- Extract public key in a separate file:  
  `openssl ec -in myKeyPair.pem -pubout -out myPublickey.pem`  

- Read public key:  
  `openssl ec -in myPublickey.pem -pubin -text -noout`  
  Public key is 65-byte number made of three parts:  
  1- the constant prefix (two bytes), e.g. "04" (this is used  when public key is compressed)  
  2- 32-byte of the P_x coordinate  
  3- 32-byte of the P_y coordinate  

- Inspect information (curve type, prime parameters ..) used to generate the key pair:  


### Application : create Bitcoin adress from Public key
- Compress oublic key:  
  `openssl ec -in myKeyPair.pem -pubout -out myCompressedPublicKey.pem -conv_form compressed`  

- Remove colon-separator from the compressed key:  
  `echo "... compressed public key .." | tr -dc [:alnum:]`  

- Hash public key using two hash function SHA256 and RIPEMD160, 20-byte hash is generated:  
  `echo 03e957c4f3e9b2f85bcb6ce033c88f2a12ee26e52eee311cc815792a374e017202 | xxd -r -p | openssl dgst -sha256 -binary | openssl dgst -ripemd160`  

- Prepend 00 (called version) to the 20-byte hash, apply sha256 twice, then take first four byte, this the cheksum:  
  `echo "00 concatenated with 20-byte hash" | xxd -r -p | openssl dgst -sha256 -binary | openssl dgst -sha256c`  

- Given the initial 20-byte hash: prepend the version number (00) to the start, and append the checksum to the end, then encode all using base58.  
  Example:  
  1- Let's 20-byte hash value is: 13f41c5aa7a7cbd7acf7b3dddfc4ccaf906f2685  
  2- checksum is: `echo "0013f41c5aa7a7cbd7acf7b3dddfc4ccaf906f2685" | xxd -r -p | openssl dgst -sha256 -binary | openssl dgst -sha256`, that gives `7d880c5c`  
  3- Append checksum `0013f41c5aa7a7cbd7acf7b3dddfc4ccaf906f26857d880c5c`, and encode to base58  
  4- Result, here is your public bitcoin id: `12pWGkwcAG9HSeFWLxWmDHcVHBXcKYguEK`  


- 

