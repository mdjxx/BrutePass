# BrutePass
Learning Password Brute force 
MDJXX
Source Code
# python 

import itertools
import string
import time

def detect_charset(password):
    if password.isdigit():
        return string.digits
    elif password.isalpha():
        if password.islower():
            return string.ascii_lowercase
        elif password.isupper():
            return string.ascii_uppercase
        else:
            return string.ascii_letters
    elif password.isalnum():
        return string.ascii_letters + string.digits
    else:
        return string.printable.strip()  # All printable chars except whitespace

def brute_force(target_password, max_length=8, max_attempts=100000000):
    charset = detect_charset(target_password)
    print(f"[*] Detected character set: {repr(charset)}")
    attempt = 0
    start_time = time.time()

    for length in range(1, max_length + 1):
        for guess_tuple in itertools.product(charset, repeat=length):
            guess = ''.join(guess_tuple)
            attempt += 1
            if guess == target_password:
                print(f"[+] Password found: {guess} (after {attempt} attempts, in {time.time()-start_time:.2f}s)")
                return guess
            if attempt >= max_attempts:
                print(f"[!] Reached maximum attempts ({max_attempts}). Stopping.")
                return None
            if attempt % 1000000 == 0:
                print(f"[*] Tried {attempt} guesses so far...")

    print("[-] Password not found within attempt limit.")
    return None

if __name__ == "__main__":
    # Example: set your password here (for demonstration)
    password = "a1B"
    # Or use: password = input("Enter the password to brute-force (for demo): ")
    brute_force(password, max_length=5, max_attempts=100000000)
