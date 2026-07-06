# 1. Create a native, unmounted folder inside the container
sudo mkdir -p ~/.ssh-local

# 2. Copy your config file and keys there so they are purely native Linux files
sudo cp /home/vscode/.ssh/config ~/.ssh-local/config
sudo cp /home/vscode/.ssh/jayvee_personal ~/.ssh-local/jayvee_personal

# 3. Set the strict, secure permissions that Linux demands (this will work now!)
sudo chmod 700 ~/.ssh-local
sudo chmod 600 ~/.ssh-local/config ~/.ssh-local/jayvee_personal

# 4. Fix the path inside your newly copied config file to point to the local key
sudo sed -i 's|~/.ssh/jayvee_personal|/home/vscode/.ssh-local/jayvee_personal|g' ~/.ssh-local/config



sudo chown -R vscode:vscode /home/vscode/.ssh-local

chmod 700 /home/vscode/.ssh-local
chmod 600 /home/vscode/.ssh-local/config /home/vscode/.ssh-local/jayvee_personal

ssh -vT -F ~/.ssh-local/config git@jayvee

git remote set-url origin git@jayvee:Jayveeeee24/mslearn-dotnet-cloudnative-devops.git

git pull --tags origin main

git pull --rebase origin main