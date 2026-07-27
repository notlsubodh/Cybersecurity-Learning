import random
import string
def generate_password(length=12):
    chars = string.ascii_letters + string.digits + string.punctuation  # Combine uppercase letters, lowercase letters, numbers, and symbols
    password =""
    for _ in range(length):  #randomly select char based on the req length
        password += random.choice(chars)
    return password
print("Welcome to the password generator")
leng = int(input("Enter the length of the password: "))
print("Your password is : " + generate_password(leng))
