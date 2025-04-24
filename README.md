# Set a predefined passphrase (⚠ WARNING: Not Secure for Production)
PASSPHRASE="YourSecurePassphraseHere"

# Function to encrypt a file
encrypt_file() {
  echo "Enter the filename to encrypt:"
  read filename
  if [ ! -f "$filename" ]; then
    echo "❌ File does not exist!"
    exit 1
  fi
  gpg --batch --yes --passphrase "$PASSPHRASE" --pinentry-mode loopback -c "$filename"
  if [ $? -eq 0 ]; then
    echo "✅ File encrypted successfully! Output: ${filename}.gpg"
  else
    echo "❌ Encryption failed!"
  fi
}

# Function to decrypt a file
decrypt_file() {
  echo "Enter the filename to decrypt (with .gpg extension):"
  read filename
  if [ ! -f "$filename" ]; then
    echo "❌ File does not exist!"
    exit 1
  fi
  gpg --batch --yes --passphrase "$PASSPHRASE" --pinentry-mode loopback -o "${filename%.gpg}" -d "$filename"
  if [ $? -eq 0 ]; then
    echo "✅ File decrypted successfully! Output: ${filename%.gpg}"
  else
    echo "❌ Decryption failed!"
  fi
}

# Function to securely delete a file
secure_delete() {
  echo "Enter the filename to securely delete:"
  read filename
  if [ ! -f "$filename" ]; then
    echo "❌ File does not exist!"
    exit 1
  fi
  shred -u "$filename"
  echo "✅ File securely deleted!"
}

# Main Menu
while true; do
  echo "🔒 Basic Security System (Google Cloud Shell)"
  echo "--------------------------------------------"
  echo "1️⃣ Encrypt a file"
  echo "2️⃣ Decrypt a file"
  echo "3️⃣ Securely delete a file"
  echo "4️⃣ Exit"
  echo "Please choose an option (1/2/3/4):"
  read choice

  case $choice in
    1) encrypt_file ;;
    2) decrypt_file ;;
    3) secure_delete ;;
    4) echo "🚪 Exiting program."; exit 0 ;;
    *) echo "❌ Invalid choice! Please try again." ;;
  esac
done
