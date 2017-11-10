MessageSafe
===========

[An HTML based message encryption tool](https://messagesafe.github.io/)

## The Problem

Whenever we have highly valuable secrets like access keys, recovery seeds, crypto currency addresses, it would be nice to secure them with a password. Unfortunately, the encryption has to be done on a computer, which is by no means a trusted medium. The secret or the password can be compromised by malicious software. 

## The Solution

MessageSafe is implemented as a single HTML file, which can be opened on any old smart phone (or tablet computer) as a web page. The phone has to be put in "airplane mode", and the encryption process performed. The resulting codes have to be photographed from the screen (by another device), and the phone has to be factory reset. If message is really valuable, this old smart phone which saw your secret can be burned in fire.

It is also wise to save the MessageSafe file (which is 'index.html') somewhere in your mailbox, so you would not be dependent on the web site for future recoveries


## Cryptography

MessageSafe employs a simple to verify encryption method, which needs a public review to determine how safe it is.

It few words, MessageSafe uses well known BCrypt function to hash passwords. BCrypt is used repeatedly on the result of prior hashing to concatenate all the hashes as a Key, which has to be as long as the Message itself.  The encryption is a simple Key XOR Message. Details are below:

Encryption

    Message = Any Ascii128 text

    Pass1, Pass2, Pass3, Pass4, Pass5 = Ascii128 based 5 password fields 

    Password = Pass1 XOR Pass2 XOR Pass3 XOR Pass4 XOR Pass5

    Checksum = {
        ShortMessage = Message 
        Repeat while ShortMessage is longer than 50 chars,
		        ShortMessage is cut in half, and the halves XOR to get new ShortMessage
        ChecksumBase = fromBase64toAscii128(BCrypt(ShortMessage XOR Password))
        Checksum = ChecksumBase.substring(1 to 4)
    }
    Key = {
        LongKey = BCrypt(Password + Checksum)
        CurrentKey = LongKey + Checksum
        Repeat while LongKey.length < Message.length,  
		        CurrentKey = BCrypt(CurrentKey)
		        LongKey = CurrentKey + LongKey
        Key = fromBase64toAscii128(LongKey)
    }

    SecureCode = Checksum + (Message XOR Key)

    SafeMessage = fromAscii128toBase32(SecureCode)
    
Decryption

     SecureCode = fromBase32toAscii128(SafeMessage)
     
     CheckSum = SecureCode.substring(1 to 4) 
		
     Password = <see in Encryption>		
     
     Key = <see in Encryption>		
		
     Message = SecureCode.substring(from 5) XOR Key

     CalculatedCheckSum = <see in Encryption>	

     Integrity check = recovered CheckSum must match CalculatedCheckSum
		
    
Note. BCrypt salt is hard coded and is removed from the resulting hash, when applying BCrypt function.
