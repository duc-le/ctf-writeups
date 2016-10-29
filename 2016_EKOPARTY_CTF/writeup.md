#The Fake Satoshi
##(misc, 300 points)

>Hello Mr. Giarc, upload again your false PGP key to pgp.mit.edu and send us any file you want with its signature to prove you are the fake Satoshi!

>The key on the server should look like the following line (case sensitive)

>Type | bits/key ID | Date | user ID
--- | --- | --- | ---
pub | 1024R/5EB7CB21 | 2008-10-30 | Fake Satoshi EKOPARTY12 (PiggyBird) satoshin@gmx.com

> http://a493e192124c317fc34511a73d63d1ae7334e132.ctf.site:40000/

The required 32-bit key ID is vulnerable to the key collision attack ([Evil 32](https://evil32.com/)), so we decided to use [Scallion](https://github.com/lachesis/scallion) to find a collision for the given 32-bit key ID. We also changed the system time to **2008-10-30** before faking the key to match the requirement from server.
 
### Finding key collision
We launched Scallion using the following command: 

    scallion.exe --gpg 5EB7CB21$

The output should look like this:
    
    Cooking up some delicions scallions...
    Using kernel optimized from file kernel.cl (Optimized4)
    Using work group size 32
    Compiling kernel... done.
    Testing SHA1 hash...
    CPU SHA-1: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    GPU SHA-1: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    Looks good!
    
    LoopIteration:1  HashCount:16.78MH  Speed:372.8MH/s  Runtime:00:00:00  Predicted:00:00:05  
    LoopIteration:2  HashCount:33.55MH  Speed:377.0MH/s  Runtime:00:00:00  Predicted:00:00:05  
    LoopIteration:3  HashCount:50.33MH  Speed:381.3MH/s  Runtime:00:00:00  Predicted:00:00:05  
    LoopIteration:4  HashCount:67.11MH  Speed:385.7MH/s  Runtime:00:00:00  Predicted:00:00:05  
    LoopIteration:5  HashCount:83.89MH  Speed:388.4MH/s  Runtime:00:00:00  Predicted:00:00:05  
    LoopIteration:6  HashCount:100.66MH  Speed:391.7MH/s  Runtime:00:00:00  Predicted:00:00:05  
    LoopIteration:7  HashCount:117.44MH  Speed:396.8MH/s  Runtime:00:00:00  Predicted:00:00:05  
    LoopIteration:8  HashCount:134.22MH  Speed:400.6MH/s  Runtime:00:00:00  Predicted:00:00:05  
    LoopIteration:9  HashCount:150.99MH  Speed:403.7MH/s  Runtime:00:00:00  Predicted:00:00:05  
    LoopIteration:10  HashCount:167.77MH  Speed:406.2MH/s  Runtime:00:00:00  Predicted:00:00:05  
    LoopIteration:11  HashCount:184.55MH  Speed:408.3MH/s  Runtime:00:00:00  Predicted:00:00:05  
    LoopIteration:12  HashCount:201.33MH  Speed:409.2MH/s  Runtime:00:00:00  Predicted:00:00:05  
    LoopIteration:13  HashCount:218.10MH  Speed:410.7MH/s  Runtime:00:00:00  Predicted:00:00:05  
    LoopIteration:14  HashCount:234.88MH  Speed:412.1MH/s  Runtime:00:00:00  Predicted:00:00:05  
    LoopIteration:15  HashCount:251.66MH  Speed:413.2MH/s  Runtime:00:00:00  Predicted:00:00:05  
    LoopIteration:16  HashCount:268.44MH  Speed:414.3MH/s  Runtime:00:00:00  Predicted:00:00:05
    Found new key! Found 1 unique keys.
    <XmlMatchOutput>
      <GeneratedDate>2008-10-30T11:10:58.7951954Z</GeneratedDate>
      <Hash>xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx5eb7cb21</Hash>
      <PrivateKey>-----BEGIN PGP PRIVATE KEY BLOCK-----
       Version: Scallion

       xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
       xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
       xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
       xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
       xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
       xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
       xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
       xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
       xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx======
       -----END PGP PRIVATE KEY BLOCK-----
      </PrivateKey>
      <PublicModulusBytes>xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx...xxxxxxx</PublicModulusBytes>
      <PublicExponentBytes>xxxxxxxx</PublicExponentBytes>
    </XmlMatchOutput>
    init: 107ms / 1 (107ms, 9.35/s)
    generate key: 900ms / 6 (150ms, 6.67/s)
    cpu precompute: 13ms / 6 (2.17ms, 461.54/s)
    total without init: 801ms / 1 (801ms, 1.25/s)
    set buffers: 0ms / 16 (0ms, 0/s)
    write buffers: 1ms / 16 (0.06ms, 16000/s)
    read results: 634ms / 16 (39.63ms, 25.24/s)
    check results: 143ms / 16 (8.94ms, 111.89/s)
    
    335.13 million hashes per second
    
    Stopping the GPU and shutting down...

We then saved the generated pgp private key block into a file called **pgp_private**

### Importing PGP key
Import the private key:

    gpg --import --allow-non-selfsigned-uid pgp_private

Output:

    gpg: key 5EB7CB21: secret key imported
    gpg: key 5EB7CB21: accepted non self-signed user ID "Scallion UID (replace me)"
    gpg: key 5EB7CB21: public key "Scallion UID (replace me)" imported
    gpg: Total number processed: 1
    gpg:               imported: 1  (RSA: 1)
    gpg:       secret keys read: 1
    gpg:   secret keys imported: 1
 
The imported key doesn't have the required name ID, so we have to fix it before signing any file.

### Key editing
Edit the recently imported key:

    gpg --edit-key 5EB7CB21
    
Output:

    gpg (GnuPG) 1.4.18; Copyright (C) 2014 Free Software Foundation, Inc.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.
    
    Secret key is available.
    
    pub  1024R/5EB7CB21  created: 2008-10-30  expires: never       usage: SCEA
                         trust: unknown       validity: unknown
    [ unknown] (1). Scallion UID (replace me)


We need to add a new user ID matching the required user ID, then delete the existing user ID "**Scallion UID (replace me)**".

At the **gpg>** prompt:

    gpg> adduid
    Real name: Fake Satoshi EKOPARTY12
    Email address: satoshin@gmx.com
    Comment: PiggyBird
    You selected this USER-ID:
        "Fake Satoshi EKOPARTY12 (PiggyBird) <satoshin@gmx.com>"
    
    Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
    
Output:

    pub  1024R/5EB7CB21  created: 2008-10-30  expires: never       usage: SCEA
                         trust: unknown       validity: unknown
    [ unknown] (1). Scallion UID (replace me)
    [ unknown] (2). Fake Satoshi EKOPARTY12 (PiggyBird) <satoshin@gmx.com>


Now we just need to delete the original user ID. First select it:

    gpg> uid 1

Output:

    pub  1024R/5EB7CB21  created: 2008-10-30  expires: never       usage: SCEA
                         trust: unknown       validity: unknown
    [ unknown] (1)* Scallion UID (replace me)
    [ unknown] (2). Fake Satoshi EKOPARTY12 (PiggyBird) <satoshin@gmx.com>

Then delete it:

    gpg> deluid 1

Output:
    
    Really remove this user ID? (y/N) y
    
    pub  1024R/5EB7CB21  created: 2008-10-30  expires: never       usage: SCEA
                         trust: unknown       validity: unknown
    [ unknown] (1). Fake Satoshi EKOPARTY12 (PiggyBird) <satoshin@gmx.com>

Finally we just save the result:

    gpg> save

### Exporting the public gpg key
Export the edited key:

    gpg --armor --export 5EB7CB21

Output:

    -----BEGIN PGP PUBLIC KEY BLOCK-----
    Version: GnuPG v1
    
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    xxxx
    xxxxx
    -----END PGP PUBLIC KEY BLOCK-----
    
We then uploaded that exported public key to [pgp.mit.edu](pgp.mit.edu).

### Signing a file
We created an  arbitrary file called test.txt. Then we signed it using the recently imported gpg key:

    gpg --output test.sign --detach-sign test.txt
    
Finally we just uploaded the **test.txt** and the **test.sign** to get the flag.
