import zipfile
import sys
import argparse

def extract_zip(zip_file, password):
    """
    Try to extract the zip file using the given password.
    Returns True if successful, False otherwise.
    """
    try:
        with zipfile.ZipFile(zip_file, 'r') as zip_ref:
            zip_ref.extractall(pwd=password.encode('utf-8'))
        return True
    except zipfile.BadZipFile:
        print(f"[!] Error: The file '{zip_file}' is not a valid ZIP file or is corrupted.")
        return False
    except RuntimeError:
        # This usually happens when the password is incorrect
        return False
    except Exception as e:
        print(f"[!] An error occurred: {e}")
        return False

def main():
    # Argument parsing for command line usage
    parser = argparse.ArgumentParser(description="Simple Zip File Password Cracker")
    parser.add_argument("zip_file", help="Path to the ZIP file")
    parser.add_argument("-w", "--wordlist", help="Path to the wordlist file (optional)", required=False)
    
    args = parser.parse_args()

    zip_file = args.zip_file
    wordlist = args.wordlist

    # Check if zip file exists
    import os
    if not os.path.exists(zip_file):
        print(f"[!] File '{zip_file}' not found.")
        sys.exit(1)

    print(f"[*] Target: {zip_file}")

    # If wordlist is provided, use it
    if wordlist:
        if not os.path.exists(wordlist):
            print(f"[!] Wordlist '{wordlist}' not found.")
            sys.exit(1)
        
        print(f"[*] Using wordlist: {wordlist}")
        try:
            with open(wordlist, 'r', encoding='utf-8', errors='ignore') as f:
                passwords = f.readlines()
        except Exception as e:
            print(f"[!] Error reading wordlist: {e}")
            return

    else:
        # If no wordlist, ask user for manual input or just say we need one
        print("[*] No wordlist provided. Entering interactive mode.")
        passwords = []
        while True:
            try:
                pwd = input("Enter password (or 'exit' to quit): ")
                if pwd.lower() == 'exit':
                    print("[*] Exiting.")
                    return
                passwords.append(pwd)
            except KeyboardInterrupt:
                print("\n[*] Exiting.")
                return

    print("[*] Starting crack...")
    found = False

    for i, password in enumerate(passwords):
        password = password.strip()
        if not password:
            continue
            
        if extract_zip(zip_file, password):
            print(f"[+] Password found: {password}")
            found = True
            break
        else:
            # Optional: Show progress if wordlist is large
            if wordlist and (i + 1) % 1000 == 0:
                print(f"[*] Tested {i + 1} passwords...")

    if not found:
        print("[-] Password not found in the provided list.")

if __name__ == "__main__":
    main()

