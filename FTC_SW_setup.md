I think I’ve got everything set up on that surface book.  I don’t know if these things would be packagable or not… but if nothing else, its a list of steps:
- Download and install latest Rev Hardware client: https://github.com/REVrobotics/REV-Software-Binaries/releases/download/rhc-1.6.6/REV-Hardware-Client-Setup-1.6.6.exe ( This page has that link: https://docs.revrobotics.com/rev-hardware-client )
- Download and install latest Android Developer studio: https://developer.android.com/studio ( have to agree to license before downloading )
- Cloned two public repos from GitHub:
 
  https://github.com/4329/IntoTheDeep.git
 
  https://github.com/acmerobotics/ftc-dashboard.git

- I just installed these public repos using HTTPS .  To use them, these machines will need GitHub accounts - maybe if they were domain accounts we could use their email to sign up? Or maybe there’s another way?

- The FTC project wouldn’t build because of class version conflicts ( can happen if the jre you run with is sufficiently newer than the compiled classes).  Noticed that this machine was running the latest Oracle JDK, so tried doing the latest AdoptOpenJDK instead.  ( https://adoptopenjdk.net/releases.html?variant=openjdk16&jvmVariant=hotspot ). Installed that from Adaptium and the FTC project built successfully after that.  Oracle is getting increasingly hostile towards open source efforts, so many open source projects have started using OpenJDK instead.
- To get FTC dashboard running, I needed to install nodeJs.  I did that using nvm ( https://github.com/nvm-sh/nvm ) to make upgrading node easier in the future:
  ```bash
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
  ```
- Then install the latest long term support of node:  nvm install —its
- To run the FTC dashboard, head into the “client” directory of the ftc-dashboard project, then to install it, run:
   ```bash
   npm i
  # Then to run it:
   npm run start
   ```