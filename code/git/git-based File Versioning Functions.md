# git-based File Versioning Functions
#profile #powershell #bash

### Initialize repo if needed
```powershell
function Initialize-GitFileTracking {
    param([string]$Path)
    if (!(Test-Path (Join-Path $Path ".git"))) {
        Set-Location $Path
        git init
        "*.tmp`n.DS_Store" | Out-File ".gitignore"
        git add .gitignore
        git commit -m "Initial commit"
    }
}
Set-Alias -Name igft -Value Initialize-GitFileTracking
```

### Save new version of file
```powershell
# Save new version of file
function Save-FileVersion {
    param(
        [Parameter(Mandatory=$true)]
        [string]$FilePath,
        [string]$Message = "Update file"
    )
    $fileInfo = Get-Item $FilePath
    Initialize-GitFileTracking $fileInfo.DirectoryName
    Set-Location $fileInfo.DirectoryName
    git add $fileInfo.Name
    git commit -m "$Message - $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')"
    Write-Host "Saved new version of $($fileInfo.Name)"
}
Set-Alias -Name sfv -Value Save-FileVersion
```

### List versions of file
```powershell
function Get-GitFileVersions {
    param([Parameter(Mandatory=$true)][string]$FilePath)
    $fileInfo = Get-Item $FilePath
    Set-Location $fileInfo.DirectoryName
    git log --follow --pretty=format:"%h %ad - %s" --date=short $fileInfo.Name
}
Set-Alias -Name gfv -Value Get-GitFileVersions
```

### Restore specific version
```powershell
function Restore-GitFileVersion {
    param(
        [Parameter(Mandatory=$true)][string]$FilePath,
        [Parameter(Mandatory=$true)][string]$CommitHash
    )
    $fileInfo = Get-Item $FilePath
    Set-Location $fileInfo.DirectoryName
    git checkout $CommitHash -- $fileInfo.Name
    Write-Host "Restored $($fileInfo.Name) to version $CommitHash"
}
Set-Alias -Name rfv -Value Restore-GitFileVersion
```

### Usage Examples

```powershell
sfv "myfile.txt" "Added feature"  # Save version
gfv "myfile.txt"                  # List versions
rfv "myfile.txt" "a1b2c3d"        # Restore version
```

```powershell
# to start tracking a file, you need to be in a repository - so initialize one
igft (Split-Path "c:\path\to\file\main.py" -Parent)

# since you're wanting to track `main.py` we'll return the parent directory for the file, and create a git repository there.

# IMPORTANT - you don't have to setup a new git repository if you already have on in the parent directory, or one if it's parent directories. 

# Save the initial version of the file
sfv "c:\path\to\file\main.py" "initial version"
```

