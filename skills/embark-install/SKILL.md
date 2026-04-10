---
name: embark-install
description: "Install the Embark CLI tool. Use when embark commands fail with 'command not found' or when the user asks to install Embark."
---

## Install Embark CLI

Run the appropriate command for the user's platform:

### macOS / Linux

```bash
curl -fsSL embark.labs.jb.gg/install.sh | bash
```

### Windows (PowerShell)

```powershell
irm https://embark.labs.jb.gg/install.ps1 | iex
```

### Errors: 401

Means the user is not logged in.
```
Call `embark login` to login 
```

### Errors: 404

Means there's no index on the server. 
```
embark index
```

## When to Use

- The `embark` command is not found
- User explicitly asks to install Embark
