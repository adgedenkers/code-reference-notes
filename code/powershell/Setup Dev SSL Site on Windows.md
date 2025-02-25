```powershell
# Variables
$domain = "dev.denkers.co"
$appRoot = "$HOME\stuff4sale"
$nginxRoot = "C:\nginx"
$sslCertName = "DevDenkersCoCert"
$hostsFile = "C:\Windows\System32\drivers\etc\hosts"
$email = "adge@denkers.co"

# Step 1: Create stuff4sale directory
Write-Host "Creating application directory at $appRoot..."
New-Item -ItemType Directory -Path $appRoot -Force

# Step 2: Download and extract Nginx for Windows (if not already installed)
if (!(Test-Path -Path $nginxRoot)) {
    Write-Host "Downloading and setting up Nginx for Windows..."
    Invoke-WebRequest -Uri https://nginx.org/download/nginx-1.24.0.zip -OutFile "$nginxRoot.zip"
    Expand-Archive -Path "$nginxRoot.zip" -DestinationPath "C:\"
    Rename-Item -Path "C:\nginx-1.24.0" -NewName "C:\nginx"
}

# Step 3: Create Nginx configuration file
$nginxConf = @"
server {
    listen 80;
    server_name $domain;

    location / {
        proxy_pass http://127.0.0.1:8501;
        proxy_http_version 1.1;
        proxy_set_header Upgrade \$http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host \$host;
        proxy_cache_bypass \$http_upgrade;
    }

    error_log logs/dev_error.log;
    access_log logs/dev_access.log;
}
"@

Set-Content -Path "$nginxRoot\conf\dev.conf" -Value $nginxConf

# Step 4: Update hosts file
if ((Get-Content $hostsFile) -notmatch [regex]::Escape("127.0.0.1 dev.denkers.co")) {
    Write-Host "Adding dev.denkers.co to hosts file..."
    Add-Content -Path $hostsFile -Value "127.0.0.1 dev.denkers.co"
} else {
    Write-Host "dev.denkers.co is already in the hosts file."
}

# Step 5: Create self-signed certificate
$cert = Get-ChildItem -Path Cert:\LocalMachine\My | Where-Object { $_.Subject -like "CN=$domain" }

if ($cert) {
    Write-Host "SSL certificate for $domain already exists."
} else {
    Write-Host "Creating a new self-signed SSL certificate for $domain..."
    $cert = New-SelfSignedCertificate -CertStoreLocation Cert:\LocalMachine\My -DnsName $domain -FriendlyName $sslCertName
    Write-Host "SSL certificate created successfully."
}

# Step 6: Run Nginx
Write-Host "Starting Nginx..."
Start-Process -FilePath "$nginxRoot\nginx.exe"

# Step 7: Run Streamlit
Write-Host "Starting Streamlit app on http://localhost:8501..."
cd $appRoot
streamlit run app.py

Write-Host "Setup for development environment is complete!"

```