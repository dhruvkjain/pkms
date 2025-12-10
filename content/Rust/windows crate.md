[[Rust/index|Rust/index]]

## Resources
[windows core processes ](https://youtu.be/B_ThnFkhJOA?si=B7XXJ6roW_OxBDSA)

## windows crate

Docs/Tutorial: https://kennykerr.ca/rust-getting-started/

`windows::Win32` corresponds to the classic C-API headers used for high-level OS interactions like "Launch a URL," "Check User Presence," or "Get Memory Usage" in a modern app context.
`windows::System` (Modern / WinRT)

#### COM (Component Object Model) API, 
COM is a Microsoft technology that provides a binary-interface standard for software components, enabling them to interact with each other across different programming languages and processes

#### Windows Filtering Platform 
WFP installs a filter at the **ALE Layer** (Application Layer Enforcement), the WFP (kernel) driver internally checks the Application ID and take actions as defined 

#### IPHelper
Mapping Ports <-> Process IDs (Local script detection)



## Content Security Policy (CSP)
docs: https://v2.tauri.app/security/csp/
CSP explanation from MDN docs: https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP

```json
// tauri.conf.json

{
  "$schema": "https://schema.tauri.app/config/2",
  "productName": "net_app_blocker",
  "version": "0.1.0",
  "identifier": "com.dkjai.net_app_blocker",
  "build": {
    "beforeDevCommand": "npm run dev",
    "devUrl": "http://localhost:1420",
    "beforeBuildCommand": "npm run build",
    "frontendDist": "../dist"
  },
  "app": {
    "windows": [
      {
        "title": "net_app_blocker",
        "width": 1280,
        "height": 960,
        "resizable": true,
        "fullscreen": false,
        "devtools": false 
      }
    ],
    "security": {
      "csp": {
        "default-src": ["'self'"],
        "connect-src": [
          "'self'", 
          "https://*.codeforces.com", 
          "https://*.codeforces.org"
        ],
        "style-src": [
          "'self'", 
          "'unsafe-inline'", 
          "https://fonts.googleapis.com"
        ],
        "font-src": [
          "'self'", 
          "https://fonts.gstatic.com"
        ]
      }
    }
  },
  "bundle": {
    "active": true,
    "targets": "all",
    "icon": [
      "icons/32x32.png",
      "icons/128x128.png",
      "icons/128x128@2x.png",
      "icons/icon.icns",
      "icons/icon.ico"
    ]
  }
}
```

## Manifest file
A manifest file in Windows is an XML document that provides critical configuration information to the operating system about how to handle an application when it is started.

Duplicate Resource error in linker: https://github.com/tauri-apps/tauri/issues/10154
The `LNK1123` error often happens because Tauri's default build process is trying to inject a manifest, and simultaneously, the linker itself is picking up another manifest from an external dependency (VS Code generates on it's own) or an older build step. This creates the `CVT1100` conflict i.e. duplicate resource

The solution: specify manifest in `app_manifest` of `build.rs` 
```rust
// build.rs

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let mut windows = tauri_build::WindowsAttributes::new();
    let manifest = include_str!("manifest.xml"); 
    
    windows = windows.app_manifest(
        manifest 
    );
    
    let attrs =  tauri_build::Attributes::new().windows_attributes(windows);
    tauri_build::try_build(attrs).expect("failed to run build script");
    
    Ok(())
}
```

```xml
<!-- manifest.xml -->
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
    <!-- admin priviledges request -->
    <trustInfo xmlns="urn:schemas-microsoft-com:asm.v3">
        <security>
            <requestedPrivileges>
                <requestedExecutionLevel level="requireAdministrator" uiAccess="false" />
            </requestedPrivileges>
        </security>
    </trustInfo>
    
    <!-- TaskDialogIndirect function part of the Windows API used to display the elevated dialogs -->
  <dependency>
    <dependentAssembly>
      <assemblyIdentity
        type="win32"
        name="Microsoft.Windows.Common-Controls"
        version="6.0.0.0"
        processorArchitecture="*"
        publicKeyToken="6595b64144ccf1df"
        language="*"
      />
    </dependentAssembly>
  </dependency>
</assembly>
```


## Allowlist (old) / Permissions and Capabilities (new)

[permissions](https://v2.tauri.app/security/permissions/) - “On-off toggles for Tauri commands”, 
scopes - “Parameter validation for Tauri commands”,
[capabilities](https://v2.tauri.app/security/capabilities/) - “Attaching permissions and scopes to Windows and WebViews”

