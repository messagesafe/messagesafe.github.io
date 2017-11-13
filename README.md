MessageSafe
===========

[An HTML based offline encryption tool for highly valuable short messages](https://messagesafe.github.io/)

## The Problem

If there is a highly valuable secret (like access key, recovery seed, crypto currency address), it is probably written on paper and is stored in a safe. It would be nice to encrypt it with a password and store it in several less secure locations, even in a mail box. Unfortunately, to do this, one would have to expose the secret to a computer, where the encryption would be performed. If the computer is infected by malicious software, the secret can be stolen. 

## The Solution

Using MessageSafe and the below routine, the secret can be encrypted securely.

1. Open MessageSafe in any old smart phone (or tablet computer) as a web page.  
2. Put the device in "airplane mode". MessageSafe is a single HTML file, it will work offline.
3. Enter the secret, the pasword (or several passwords), and press Encrypt
4. Press "Enlarge to make a photograph" button to show large encrypted codes.
4. Make a picture of the codes with your other phone, this is the secure picture you want to store.
5. Without connecting to anywhere, go to settings and do a complete "Factory Reset" for the first device. (If the secret is really valuable, the device can even be physically destroyed)

For the decryption the same routine can be followed. 

Note, if you do use MessageSafe, it is wise to save its single HTML file in your mail box, in order to ensure future independent decryptions.

## Cryptography

MessageSafe uses third party JavaScript implementations of standard security protocols 

Encryption

    Message = Any Ascii128 text

    Pass1, Pass2, Pass3, Pass4, Pass5 = Ascii128 based 5 password fields, with white spaces removed

    Password =  Concatenate(Sort(Pass1, Pass2, Pass3, Pass4, Pass5))

    Key = SHA256(BCrypt(SHA256(Password)))		

    Code = AES.encrypt(Message, Key)
    
    SafeMessage = fromAscii256toBase32(SecureCode)
    
Decryption

     Code = fromBase32toAscii256(SafeMessage)
     
     Key = <see in Encryption>		
	
     Message = AES.decrypt(Code, Key)
    
    
Note. There is no checksum, as not required for the intended use

Note. BCrypt salt is just a hash of the password, what is equivalent to no salt used. AES is employed in CBC mode with hardcodded initialization array. This is due to the requirement to have the same output for the same input and password. 





