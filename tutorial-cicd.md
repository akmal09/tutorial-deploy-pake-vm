### 1. Prepare your VM
Before GitHub Actions can deploy your code, the VM must allow SSH connections from GitHub.

## 2. Create an SSH Key on Your Local PC:
```cli
ssh-keygen -t ed25519 -C "github-actions"
```
pilih lokasi file yang sesuai.


## 3. Copy the public key ({LOCAL_PC_DIRECTORY}/.ssh/id_ed25519.pub) into your VM's ~/.ssh/authorized_keys:

## 4. Test SSH without password:
```cli
ssh youruser@your-vm-ip
```

## 5. config supaya command sudo tertentu bisa di eksekusi sama github action atau platform cicd lainnya.
sudo visudo
