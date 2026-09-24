# Mathematics concept's 
Terms with an analogy of vending machine
"Domain","All possible inputs you can give","All the buttons on the machine"
"Codomain","All possible outputs the machine could give","All drinks that exist in the world (the full list the machine is designed to potentially serve)"
"Range","The actual outputs you do get","Only the drinks that are actually in the machine's slots"

"Type","No Two Inputs Share Output?","All Codomain Values Used?","Has Inverse?","Cybersecurity Use"
"Injective","✅","❌ (may have unused)","Yes (on range)","Perfect hashing, unique IDs"
"Surjective","❌ (collisions allowed)","✅","No","Cryptographic hashes (SHA-256)"
"Bijective","✅","✅","✅ (full inverse)","Encryption (AES, RSA)"
"Neither","❌","❌","No","General hash maps"

  # The Pigeonhole Principle (Critical for Understanding Hashing)
If you have n pigeons and m holes where n > m, at least one hole must contain more than one pigeon.
 # in cyber terms:
SHA-256 takes any input (infinite possibilities) → produces a 256-bit output (only 2²⁵⁶ possible values)
Since inputs > outputs, collisions are mathematically guaranteed
This is why SHA-256 is NOT injective and NOT bijective
But we don't care! We just need it to be very hard to find a collision (computational infeasibility) 
  # connections
 # Hash Functions (Surjective, NOT Injective)
Input (any length)  →  [SHA-256]  →  Fixed 256-bit output
"hello"            →  "2cf24dba5fb0a30e26e83b2ac5b9e29e..."
"hello!"           →  "9f86d081884c7d659a2feaa0c55ad015..."

Domain: All possible strings (infinite)
Codomain: All 256-bit values (2²⁵⁶ ≈ 1.16 × 10⁷⁷ values)
Range: (Theoretically) all 256-bit values → Surjective
Injective? NO — by pigeonhole principle, two different inputs MUST produce the same hash
Why it's still useful: Finding a collision is computationally infeasible (would take longer than the age of the universe)
# Real Use Cases:
Verifying file integrity (download a file, hash it, compare with published hash)
Storing passwords (never store plain text, store the hash)
Digital signatures
# Encryption Functions (Bijective)
Plaintext + Key  →  [AES-256]  →  Ciphertext
Ciphertext + Key →  [AES-256⁻¹] →  Plaintext

Domain: All 128-bit (or 256-bit) blocks
Codomain: All 128-bit (or 256-bit) blocks
Range = Codomain (every possible ciphertext can be produced)
Bijective? YES — this is required for decryption to work
Key insight: The key makes the function look random, but the function itself is a permutation (bijective) 
Why bijective is essential:

If AES were NOT injective:
  AES("ATTACK AT DAWN", key) = "XK92..."
  AES("MEET AT NOON", key)   = "XK92..."  ← COLLISION!
  
  When you decrypt "XK92...", you can't know which message was sent!
  → Security completely broken

# Public-Key Cryptography (RSA) — One-Way Functions
RSA uses a function that is:

Easy to compute (encryption): c = m^e mod n
Hard to invert (decryption without the key): m = c^d mod n 
Mathematically, for a fixed key, RSA encryption is bijective on the set of valid messages. But finding the inverse requires factoring n = p × q, which is computationally infeasible for large primes. 

This is called a one-way trapdoor function:

Easy direction: ✅
Reverse direction without the "trapdoor" (private key): ❌ (infeasible)
Reverse direction WITH the trapdoor: ✅

# Access Control as a Function
def authorize(user: str, resource: str) -> bool:
    # Domain: all (user, resource) pairs
    # Codomain: {True, False}
    # This is a SURJECTIVE function (both True and False are possible)
    # It is NOT injective (many user/resource pairs give the same answer)
    permissions = {
        "alice": ["/home", "/admin"],
        "bob": ["/home"]
    }
    return resource in permissions.get(user, [])

# Firewall Rule as a Function
f: (source_ip, port, protocol) → {ALLOW, DROP}

# Domain: all possible (IP, port, protocol) combinations
# Codomain: {ALLOW, DROP}
# Range: {ALLOW, DROP} (both are possible) → Surjective
# NOT injective: many different packets get the same decision

# Problem: Why SHA-256 Cannot Be Bijective
Given: SHA-256: {0,1}* → {0,1}²⁵⁶ 

Solution:

Domain: all possible binary strings of ANY length → infinite (countably infinite, ℵ₀)
Codomain: all 256-bit strings → finite (2²⁵⁶ elements)
By the Pigeonhole Principle: if domain is larger than codomain, the function CANNOT be injective
Therefore it CANNOT be bijective (bijective requires injective)
Conclusion: SHA-256 is surjective (by design) but NOT injective, hence NOT bijective 

 # Problem 4: Why AES Must Be Bijective
Given: AES-128 takes a 128-bit block and produces a 128-bit block.

Solution:

Domain: {0,1}¹²⁸ (2¹²⁸ possible inputs)
Codomain: {0,1}¹²⁸ (2¹²⁸ possible outputs)
For decryption to work, we need: for every ciphertext c, there is exactly one plaintext m such that AES(m, key) = c
This requires the function to be bijective
Since domain and codomain are the same finite size, injective ⟹ surjective ⟹ bijective
AES is a pseudorandom permutation = a bijective function that looks random 

# Problem : Password Hashing — Why We Don't Store Passwords
Scenario: A website stores 1,000,000 user passwords.

Naive approach (BAD): Store passwords in plain text.

If database is breached → all passwords exposed
Correct approach: Store f(password) where f = SHA-256(password + salt)

User enters: "MyP@ssw0rd123"
System stores: a3f5b8c9d1e2f4a6b7c8d9e0f1a2b3c4...  (64 hex chars)

Math analysis:

f is a one-way function (easy to compute, infeasible to reverse)
f is NOT injective in theory (collisions exist), but finding them is infeasible
Salt makes each user's hash unique even if they have the same password:
f("password" + "salt_abc") ≠ f("password" + "salt_xyz") 

# Problem : Firewall Function — Surjective Analysis
Given: A firewall function f: P → {ALLOW, DROP} where P is the set of all possible network packets.

Questions:

Is f injective? NO — millions of different packets get the same "ALLOW" decision
Is f surjective? YES — both ALLOW and DROP are actually used
Is f bijective? NO — not injective
Why this is fine: We don't need to reverse a firewall decision. We just need the correct decision for each packet. 

# Problem: Digital Signature — Using Bijective + Hash
Process:

Message M → Hash H = SHA-256(M) [surjective, not injective]
H → Signed = RSA_Encrypt(H, private_key) [bijective]
Verifier: RSA_Decrypt(Signed, public_key) = H' [inverse of bijective]
Check: SHA-256(M) = H'? → Integrity verified! 
Why both are needed:

Hash: compresses arbitrary-length message to fixed size (can't RSA-encrypt a 10GB file)
RSA (bijective): provides the "one-way with trapdoor" property for authentication

# Problem : Finding Domain in Programming
import math

def calculate_credential_strength(password: str) -> float:
    # What's the domain? What's the codomain? What's the range?
    length_score = len(password) * 4
    upper_score = sum(1 for c in password if c.isupper()) * 2
    digit_score = sum(1 for c in password if c.isdigit()) * 3
    special_score = sum(1 for c in password if not c.isalnum()) * 5
    return math.sqrt(length_score + upper_score + digit_score + special_score)

Analysis:

Domain: All strings (technically all of str)
Codomain: All floats (return type is float)
Range: [0, ∞) — actually all non-negative reals that are square roots of non-negative integers
Is it injective? NO — "Ab1!" and "Ba1!" both have the same scores → same output
Is it surjective? NO — can't produce negative values

