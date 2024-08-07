I am trying to make a Straddling Checkerboard for a OneTimePad encryptions system in Python, can you help me build it?

# Straddling Checkerboard

1. ## Is the primary purpose of the `Straddling_Checkerboard` class to generate a conversion table for one-time pad encryption/decryption, or does it have other functionalities as well?
   
   the straddling checkerboard class should be seperate from the onetimepad encryption and decryption but we are primarily using it with onetimepad.

2. ## Are there any specific rules or patterns for arranging the characters in the checkerboard layout?
   
   The rules are, we need to expand the digits(in the numerical example) to fit with a different sized plaintext_alphabet. for example when it goes from T: 6 to C:71. The idea of this is we need to preserve the numbers we have not used yet like 7, so we can expand on them so we get 71, 72, etc. That way we dont get encoding/decoding errors, because as we scan through the text we can do an easy replace to convert it, without conflict, do let me know if you don't understand this concept.
   Here is an example(our version will be in alphabetical order most likely)
   
   CONVERSION TABLE NO.1 EN
   CODE-0 B-70 P-80 LITT-90
   A-1 C-71 Q-81 (.)-91
   E-2 D-72 R-82 (:)-92
   I-3 F-73 S-83 (')-93
   N-4 G-74 U-84 ( )-94
   O-5 H-75 V-85 (+)-95
   T-6 J-76 W-86 (-)-96
   K-77 X-87 (=)-97
   L-78 Y-88 REQ-98
   M-79 Z-89 SPC-99

The only reserved Conversion table entries are CODE:0 (Meaning Codebook), LITT(Meaning Literal):90 and SPC(Meaning Space):99.
The LITT keyword will mean the next digits after LITT(99) are literal digits and to be treated as part of the message in its raw form and its ended with another LITT(99).

Note: the numbers like 99,0,90,etc are just placeholders, they will be dynamically adjusted based on the ciphertext_alphabet used.

3. ## How would the codebook be integrated into the class? Should it be a separate input or stored within the class?
   
   the codebook should be a separate list of strings specified by the user.

4. ## How do you envision the usage of the `StraddlingCheckerboard` class? Could you provide an example scenario or use case?

The usage of the straddling checkerboard
here is a quote from wikipedia:
"A straddling checkerboard is a device for converting an alphanumeric plaintext into digits whilst simultaneously achieving fractionation (a simple form of information diffusion) and data compression relative to other schemes using digits. It also is known as a monôme-binôme cipher. "

Our Straddling Checkerboard will work slightly differently, the user has free will to choose any characterset they would like. But the main idea is the same, we need to achieve data compression in that we use the least number of characters possible to convey a message.

here is some example usage:

```python
plaintext_alphabet = list("abcdefghijklmnopqrstuvwxyz")
ciphertext_alphabet = list("0123456789")
codebook = ["ATTACK", "DEFEND","BACK TO BASE","RETREAT", "HELLO"]
checkerboard = StraddlingCheckerboard(plaintext_alphabet=plaintext_alphabet, ciphertext_alphabet=ciphertext_alphabet, codebook=codebook)
""" checkerboard.conversion_table
{ '010':'ATTACK','020':'DEFEND', '030': 'BACK TO BASE', '040': 'RETREAT', '050':'HELLO', '1': 'a', '2': 'b',
 '3': 'c', '4': 'd', '5': 'e',
 '6': 'f', '70': 'g', '71': 'h',
 '72': 'i', '73': 'j', '74': 'k',
 '75': 'l', '76': 'm', '77': 'n',
 '78': 'o', '79': 'p', '80': 'q',
 '81': 'r', '82': 's', '83': 't',
 '84': 'u', '85': 'v', '86': 'w',
 '87': 'x', '88': 'y', '89': 'z', '90': '[LITERAL HERE PUT EMPTY STRING]', '91': ' '}
"""
encoded_msg = checkerboard.encode("HELLO world 100") # Encodes to "050918678817549010090"
decoded_msg = checkerboard.decode("050918678817549010090") # Decodes to "HELLO world 100"" 
```

here is another example with the codebook expanded:

```python
plaintext_alphabet = list("abcdefghijklmnopqrstuvwxyz")
ciphertext_alphabet = list("0123456789")
codebook = ["ATTACK", "DEFEND","BACK TO BASE","RETREAT", "HELLO", "JAMES", "BOB", "TOM", "CALLOUT", "CALLIN"]
checkerboard = StraddlingCheckerboard(plaintext_alphabet=plaintext_alphabet, ciphertext_alphabet=ciphertext_alphabet, codebook=codebook)
""" checkerboard.conversion_table
{ '010':'ATTACK','020':'DEFEND', 
'030': 'BACK TO BASE', '040': 'RETREAT', 
'050':'HELLO', '060':'JAMES','070':'BOB','080':'TOM','090':'CALLOUT','0100':'CALLIN',
 '1': 'a', '2': 'b',
 '3': 'c', '4': 'd', '5': 'e',
 '6': 'f', '70': 'g', '71': 'h',
 '72': 'i', '73': 'j', '74': 'k',
 '75': 'l', '76': 'm', '77': 'n',
 '78': 'o', '79': 'p', '80': 'q',
 '81': 'r', '82': 's', '83': 't',
 '84': 'u', '85': 'v', '86': 'w',
 '87': 'x', '88': 'y', '89': 'z', '90': '[LITERAL HERE PUT EMPTY STRING]', '91': ' '}
"""
encoded_msg = checkerboard.encode("HELLO james7 DEFEND") # Encodes to "050910609079091020"
decoded_msg = checkerboard.decode("050910609079091020") # Decodes to "HELLO james7 DEFEND" 
```

5. ## What should the code look like in structure?
   
   ```python
   class StraddlingCheckerboard:
       def __init__(self, plaintext_alphabet: str, ciphertext_alphabet: str, codebook: Optional[list[str] = []]):
           self.conversion_table = self.__generate(plaintext_alphabet,ciphertext_alphabet,codebook)
   
       @staticmethod
       def __generate(plaintext_alphabet: str, ciphertext_alphabet: str, codebook: Optional[list[str] = []]) -> dict:
           # Code here...
           return conversion_table
   
       def encode(self, msg):
           # Code here...
   
       def decode(self, code):
           # Code here...
   ```
