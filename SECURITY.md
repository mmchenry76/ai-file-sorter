# Security Policy

## Supported Versions

This is a personal fork of [hyperfield/ai-file-sorter](https://github.com/hyperfield/ai-file-sorter). Security fixes are applied to the latest version on the `main` branch only.

| Version | Supported |
| ------- | --------- |
| main (latest) | :white_check_mark: |
| older commits | :x: |

## Reporting a Vulnerability

If you discover a security vulnerability in this repository, please report it privately using GitHub's built-in vulnerability reporting:

1. Go to the **Security** tab of this repository.
2. 2. Click **Report a vulnerability**.
   3. 3. Fill in the details and submit.
     
      4. Do **not** open a public issue for security vulnerabilities.
     
      5. You can expect an initial response within **7 days**. If the vulnerability is confirmed, a fix will be prioritised and a patched version released as soon as possible.
     
      6. ## Scope
     
      7. This repository contains a C++ desktop application. Areas of particular security concern include:
     
      8. - File system access and path traversal
         - - Input validation and sanitisation
           - - Third-party dependency vulnerabilities (monitored via Dependabot)
             - - Secrets or credentials accidentally committed (monitored via Secret Scanning)
              
               - ## Out of Scope
              
               - - Vulnerabilities in the upstream project ([hyperfield/ai-file-sorter](https://github.com/hyperfield/ai-file-sorter)) should be reported directly to the upstream maintainer.
                 - - General bugs or feature requests should be opened as regular GitHub Issues.
