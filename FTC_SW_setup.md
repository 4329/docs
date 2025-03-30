Here's steps followed to get a machine configured for software on the 2024/2025 IntoTheDeep FTC season:
- Download and install latest Rev Hardware client: https://github.com/REVrobotics/REV-Software-Binaries/releases/download/rhc-1.6.6/REV-Hardware-Client-Setup-1.6.6.exe ( This page has that link: https://docs.revrobotics.com/rev-hardware-client )
- Download and install latest Android Developer studio: https://developer.android.com/studio ( have to agree to license before downloading )
- Cloned two public repos from GitHub:
 
  https://github.com/4329/IntoTheDeep.git
 
  https://github.com/acmerobotics/ftc-dashboard.git

- I just installed these public repos using HTTPS .  To use them, these machines will need GitHub accounts - maybe if they were domain accounts we could use their email to sign up? Or maybe there’s another way?

- The FTC project wouldn’t build because of class version conflicts ( can happen if the jre you run with is sufficiently newer than the compiled classes).  Noticed that this machine was running the latest Oracle JDK, so tried doing the latest AdoptOpenJDK instead.  ( https://adoptopenjdk.net/releases.html?variant=openjdk16&jvmVariant=hotspot ). Installed that from Adaptium and the FTC project built successfully after that.  Oracle is getting increasingly hostile towards open source efforts, so many open source projects have started using OpenJDK instead.