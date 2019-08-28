### What 
Generate public/private keys using Elliptic Curve Cryptography (ECC)

### Why
Discrete logarithm problem becomes hard when using Elliptic Curves.  
High level steps:  
- Special Elliptic Curve definition: y² = x³+ax+b.  
- take a random number with 256-bit : n, this the private key.  
- Take a point G(x, y) on the elliptic curve.  
- Multiply both P(P_x, P_y) = n  * G(x, y). P(P_x, P_y) is the public key  
 


### How (using openssl)
- Select an elliptic curve (NIST recommands a bunch of them). The most popula is "Secp256k1" : y² = x³+7  

- Generate key-pairs: 
  openssl ecparam -name secp256k1 -genkey -out myKeyPair.pem

- myKeyPair.pem (base64 encoded file) contains the private key, the public key and all used information (e.g. secp256k1 ..)  

- Read kay pair information: 
  openssl ec -in myKeyPair.pem -text -noout  

- Extract public key in a separate file: 
  openssl ec -in myKeyPair.pem -pubout -out myPublickey.pem  

- Read public key: openssl ec -in myPublickey.pem -pubin -text -noout
  Public key is 65-byte number made of three parts:  
  1- the constant prefix (two bytes), e.g. "04"  
  2- 32-byte of the P_x coordinate  
  3- 32-byte of the P_y coordinate  

- Inspect information (curve type, prime parameters ..) used to generate the key pair:  