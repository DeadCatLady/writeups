NAME: not_very_secure
CATEGORY: crypto
POINTS: 300
DOWNLOAD LINK:

[http://chal.ctf-league.osusec.org/not_very.py](http://chal.ctf-league.osusec.org/not_very.py)

ACCESS: None
DESCRIPTION: RSA is a Really Secure Algorithm when used correctly. Here it isn't...

```
N: 4777805290463711026025530760997341215210485822996023118027623646117723191500719041518959799725566059428554801696706296582581695310479723757513175744877550944865803932093280029829366260565558093216826465112625300559200066643691564027696443851265545121928906737207782768662058439770013206909817779391660295937449587329571897991774335797160269734366923630381643014686236264025697244867112514868261836050971328293396853758777800827880833925181379721342353017797906724353979687889041742404605317986943822242304998601391883361325971913770331586178253989604353973935125868269417765045588838194560248401591031608094124211
E: 11
The encrypted flag in base 64: SOIBDfTgLGiKSogVGF1ell/EJNthxiL+rP7QjMjg4j4l58piOWEnF7oDQMAc3y3QhXHBC4RU4TsemCENzTae1zpBJ5W3XmwbBvF8ot19E28FVBjZLE5uUk7caH8b1q/2GhZQnLNtfHHHZzlFcvg5ENiA1iqlpxoO+VLcgLqs2zpDFihamaGLOA0I1yC/vwtn79rgg3UMJVikFqlrBMdN2h3WuMKwPB9vCfjXI+XrhPDRr96rO5xKVPzQvjJSu4Rz3jsKbz0WmnNE7lmNSZDi+P+KKBFZffJWKRaIwEWJQl8y/4yFjz1rHhX/ta2mPVEEBfO8sM/oc3UPp8E2BKAB
```

```python
#!/usr/bin/python3
from sys import path 
# from pwn import *
from base64 import b64decode
from sympy import nextprime, prevprime, isprime
from math import isqrt

"""
# RSA encoding/decoding step
def enc(key, data):
    dataInt = int.from_bytes(data, 'little')
    encData = pow(dataInt, key[1] , key[0])
    return encData.to_bytes(encData.bit_length()//8+1, 'little')
    
# Test regular decrypt
# dec_flag = enc((n,d),enc_flag)
# print(dec_flag.decode())

p = process('./')

print(p.recvline().decode())
n = int(p.recvline())
print(n)
print()
print(p.recvline().decode())
e = int(p.recvline())
print(e)
print()
print(p.recvline().decode())
rec_flag = (p.recvline())
print(rec_flag.decode())
enc_flag = b64decode(rec_flag)

print()

# # Example solve (n is prime instead of product of primes)
found_d = pow(e, -1, n-1)
print("D found to be:")
print(found_d)
dec_flag = enc((n,found_d),enc_flag)
print()
print("Flag decrypted as:")
print(dec_flag.decode())
"""

n = 4777805290463711026025530760997341215210485822996023118027623646117723191500719041518959799725566059428554801696706296582581695310479723757513175744877550944865803932093280029829366260565558093216826465112625300559200066643691564027696443851265545121928906737207782768662058439770013206909817779391660295937449587329571897991774335797160269734366923630381643014686236264025697244867112514868261836050971328293396853758777800827880833925181379721342353017797906724353979687889041742404605317986943822242304998601391883361325971913770331586178253989604353973935125868269417765045588838194560248401591031608094124211
sqr = isqrt(n) 

p = prevprime(sqr)
print(p)

# Get next
q = nextprime(p)

t = p * q
# If greater than sqr, 
while True:
    if t == n:
        print(p, "\n", q)
        break
    elif t < n:
        q = nextprime(q)
    else:
        p = prevprime(p)

    t = p * q

phi = (p-1) * (q-1)

d = pow(11, -1, phi)
print(d)

m = b64decode(b'SOIBDfTgLGiKSogVGF1ell/EJNthxiL+rP7QjMjg4j4l58piOWEnF7oDQMAc3y3QhXHBC4RU4TsemCENzTae1zpBJ5W3XmwbBvF8ot19E28FVBjZLE5uUk7caH8b1q/2GhZQnLNtfHHHZzlFcvg5ENiA1iqlpxoO+VLcgLqs2zpDFihamaGLOA0I1yC/vwtn79rgg3UMJVikFqlrBMdN2h3WuMKwPB9vCfjXI+XrhPDRr96rO5xKVPzQvjJSu4Rz3jsKbz0WmnNE7lmNSZDi+P+KKBFZffJWKRaIwEWJQl8y/4yFjz1rHhX/ta2mPVEEBfO8sM/oc3UPp8E2BKAB')

m_int = int.from_bytes(m, 'little')

print(m_int)

decrypted_int = pow(m_int, d, n)
m = decrypted_int.to_bytes(decrypted_int.bit_length()//8+1, 'little')

print(m)
```
