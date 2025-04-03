# Bandit15 → 16: SSL Handshake to Victory

[Challenge](https://overthewire.org/wargames/bandit/bandit16.html)

## Level Description

In Bandit15, the challenge involves retrieving the password for Bandit16 by interacting with a service running on `localhost` at port `30001`. The service uses SSL encryption, so we need to establish a secure connection to communicate with it. The goal is to send the current level’s password to this service and receive the password for Bandit16 in return.

## The Process

I started by logging into the Bandit15 level using SSH with the password from the previous level:

1. **Understanding the Task:**
    
    The challenge requires us to interact with a service on **`localhost:30001`** using SSL. To do this, I needed to use the **`openssl`** command-line tool, specifically the **`s_client`** module, which allows us to connect to an SSL server.
    
2. **Exploring OpenSSL:**
    
    I began by checking the available commands in **`openssl`**:
    
    ```bash
    bandit15@bandit:~$ openssl
    help:
    
    Standard commands
    asn1parse         ca                ciphers           cmp
    
    cms               crl               crl2pkcs7         dgst
    
    dhparam           dsa               dsaparam          ec
    
    ecparam           enc               engine            errstr
    fipsinstall       gendsa            genpkey           genrsa
    help              info              kdf               list
    
    mac               nseq              ocsp              passwd
    pkcs12            pkcs7             pkcs8             pkey
    
    pkeyparam         pkeyutl           prime             rand
    
    rehash            req               rsa               rsautl
    s_client          s_server          s_time            sess_id
    smime             speed             spkac             srp
    
    storeutl          ts                verify            version
    x509
    
    Message Digest commands (see the `dgst' command for more details)
    blake2b512        blake2s256        md4               md5
    
    rmd160            sha1              sha224            sha256
    sha3-224          sha3-256          sha3-384          sha3-512
    sha384            sha512            sha512-224        sha512-256
    shake128          shake256          sm3
    
    Cipher commands (see the `enc' command for more details)
    aes-128-cbc       aes-128-ecb       aes-192-cbc       aes-192-ecb
    aes-256-cbc       aes-256-ecb       aria-128-cbc      aria-128-cfb
    aria-128-cfb1     aria-128-cfb8     aria-128-ctr      aria-128-ecb
    aria-128-ofb      aria-192-cbc      aria-192-cfb      aria-192-cfb1
    aria-192-cfb8     aria-192-ctr      aria-192-ecb      aria-192-ofb
    aria-256-cbc      aria-256-cfb      aria-256-cfb1     aria-256-cfb8
    aria-256-ctr      aria-256-ecb      aria-256-ofb      base64
    bf                bf-cbc            bf-cfb            bf-ecb
    bf-ofb            camellia-128-cbc  camellia-128-ecb  camellia-192-cbc
    camellia-192-ecb  camellia-256-cbc  camellia-256-ecb  cast
    cast-cbc          cast5-cbc         cast5-cfb         cast5-ecb
    cast5-ofb         des               des-cbc           des-cfb
    des-ecb           des-ede           des-ede-cbc       des-ede-cfb
    des-ede-ofb       des-ede3          des-ede3-cbc      des-ede3-cfb
    des-ede3-ofb      des-ofb           des3              desx
    rc2               rc2-40-cbc        rc2-64-cbc        rc2-cbc
    rc2-cfb           rc2-ecb           rc2-ofb           rc4
    rc4-40            seed              seed-cbc          seed-cfb
    seed-ecb          seed-ofb          sm4-cbc           sm4-cfb
    sm4-ctr           sm4-ecb           sm4-ofb
    ```
    
    This gave me a list of commands, and I noticed **`s_client`**, which is used for establishing SSL/TLS connections.
    
    1. **Connecting to the Service:**
        
        Using the **`s_client`** module, I connected to the service running on **`localhost:30001`** :
        
        ```bash
        bandit15@bandit:~$ openssl s_client -connect localhost:30001
        CONNECTED(00000003)
        Can't use SSL_get_servername
        depth=0 CN = SnakeOil
        verify error:num=18:self-signed certificate
        verify return:1
        depth=0 CN = SnakeOil
        verify return:1
        ---
        [REDACTED]
        ---
        read R BLOCK
        ---
        [REDACTED]
        ---
        read R BLOCK
        ```
        
        This command initiated an SSL handshake with the server. After the handshake, the connection was established, and I was ready to interact with the service. 
        
    2. **Sending the Password:**
        
        The service was waiting for input, so I pasted the current level’s password into the terminal. After sending the password, the server responded with:
        
        ```bash
        [REDACTED]
        Correct!
        [REDACTED]
        
        closed
        ```
        
        Voila! Beautiful password and our golden ticket has appear!
        

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **SSL/TLS Basics:** I learned how to use the `openssl` command-line tool to establish a secure connection with an SSL server.
- **Interacting with Services:** I gained experience in sending data to a service over an encrypted connection and retrieving the response.
- **Debugging SSL Connections:** The **`s_client`** module provided detailed information about the SSL handshake, certificates, and encryption protocols, which was helpful for understanding the process.

## Helpful Reading Material

- [Testing Openssl](https://www.feistyduck.com/library/openssl-cookbook/online/testing-with-openssl/index.html)
- [Practical uses of Openssl command](https://www.geeksforgeeks.org/practical-uses-of-openssl-command-in-linux/)
- [S_Client](https://linux.die.net/man/1/s_client)
- [Openssl S_Client](https://www.misterpki.com/openssl-s-client/#google_vignette)
