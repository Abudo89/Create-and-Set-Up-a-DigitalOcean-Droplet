# Create-and-Set-Up-a-DigitalOcean-Droplet


1. Create and Set Up a DigitalOcean Droplet
Go to DigitalOcean and sign up (or log in).
Click the “Create” button at the top right and select Droplets.
Choose an OS: Ubuntu 22.04 LTS is recommended.
Select a plan: Use "Basic" (the $5 or $6/month plan is enough for most React apps).
Choose a data center region close to your users.
Either add your SSH key (recommended) or choose a root password.
Click “Create Droplet”.
2. Connect to Your Droplet
On your local machine, open the terminal and connect via SSH:

ssh root@YOUR_DROPLET_IP
(Replace YOUR_DROPLET_IP with the actual IP shown in the DigitalOcean dashboard.)

3. Set Up Node.js and Git on the Droplet
a) Install Node.js:


# Install Node Version Manager (nvm)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

# Reload shell
source ~/.bashrc

# Install the latest LTS version of Node.js
nvm install --lts

# Verify
node -v
npm -v
b) Install Git:


apt update
apt install git -y
4. Clone Your Project's Code
Your code is in a GitHub repo (linked to Lovable).
Clone it:

git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
cd YOUR_REPO
5. Install Project Dependencies

npm install
6. Build Your App for Production

npm run build
7. Serve the App
For a quick test:


npm install -g serve
serve -s dist -l 3000
Your app will be available at http://YOUR_DROPLET_IP:3000

8. (Optional) Set Up a Process Manager and Reverse Proxy
For production, it’s best to use a reverse proxy like Nginx with a process manager or static hosting.
a) Install Nginx:

apt install nginx -y
b) Configure Nginx to Serve Your App (sample config)**

nano /etc/nginx/sites-available/default
Replace the content with:

server {
  listen 80;
  server_name _;

  root /root/YOUR_REPO/dist;
  index index.html;

  location / {
      try_files $uri $uri/ /index.html;
  }
}
Save and exit (Ctrl+O, Enter, Ctrl+X)
Reload Nginx:

systemctl reload nginx
c) Access Your App
Go to http://YOUR_DROPLET_IP in your browser.

9. Set Environment Variables
If your project uses .env files (for keys, etc):

Upload them to the server or create manually using nano .env in your project root.
10. Optional: Set Up Domain and SSL
Point your domain’s DNS A record to your droplet’s IP.
Use Certbot to set up free SSL via Let's Encrypt.
11. Keeping the App Running
If you’re serving with Node.js (not just static), use pm2 or similar to keep your app running after logout.

12. Deploy Updates
Pull new code with git pull
Rebuild: npm install && npm run build


