# The Best RSA
## 250

At his last rally, Trump made an interesting statement:

> I know RSA, I have the best RSA
The more bits I have, the more secure my cyber, and my modulus is YUUUUUUUUUUUUUGE
We don't believe his cyber is as secure as he says it is. See if you can break it for us

best_rsa

author's irc nick: krx

### Overview
The given text file contains 3 number: the public exponent **e** (65537), the modulus **n** (about 500K bits) and the cipher text **c** (about 500K bits). The challenge is to factorize that "YUUUUUUUUUUUUUGE" modulus. Let's look at its very last digits:

> n = 6423578....867187**5**

It ends with the digit **5**, so it is divisible by 5! We did a quick check and found out that it is also divisible by 3, 7, 11, 13 ... Then we tried to factorize it into several first prime numbers and luckily, we succeeded with a very small effort :)

### Finding the private exponent d
By factorizing the modulus **n** into primes p1, p2, p3 ... we could use the [Euler's totient function](https://en.wikipedia.org/wiki/Euler%27s_totient_function) and the public exponent **e** to calculate the private exponent **d**:

> d = inv(e) (mod phi(N))

where phi is the Euler's totient function.

### Decrypt the cipher text
This seems to be the easiest part, but it took us several hours to perform the calculation to decrypt the given cipher text:

> plain_text = c<sub>e</sub> (mod n)

Here's our [script](https://github.com/duc-le/ctf-writeups/blob/master/2016_Hack_The_Vote/crypto/250_The_Best_RSA/solve_best_rsa.py) to factorize **n**, decrypt **c** and save the plaintext to a given file.
The result is actually a gif file:

![flag](https://github.com/duc-le/ctf-writeups/blob/master/2016_Hack_The_Vote/crypto/250_The_Best_RSA/flag.gif)

### The flag
**flag{s4ved_by_CH1N4_0nc3_aga1n}**
