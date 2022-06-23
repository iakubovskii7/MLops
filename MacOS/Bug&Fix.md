
**Try to install scrypt==0.8.20 via poetry**
*Error*
***scrypt-1.2.1/libcperciva/crypto/crypto_aes.c:6:10: fatal error: 'openssl/aes.h' file not found
        #include <openssl/aes.h>***

***Solution***
env LDFLAGS="-L$(brew --prefix openssl)/lib" CFLAGS="-I$(brew --prefix openssl)/include" pip install scrypt

