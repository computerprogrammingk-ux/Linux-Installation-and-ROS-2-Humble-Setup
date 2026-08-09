# Linux Installation and ROS 2 Humble Setup

**Complete Installation Report**

| Item | Details |
| --- | --- |
| Operating System | Ubuntu 22.04.5 LTS (Jammy Jellyfish) |
| Virtualization | VMware Workstation |
| ROS Distribution | ROS 2 Humble |
| Architecture | 64-bit (amd64) |
| Final Status | Installed and verified successfully |

---

## 1. Objective

The objective of this task was to install a Linux operating system, configure ROS 2 Humble, verify that ROS is working correctly, and document the complete process. **Ubuntu 22.04.5 LTS** was selected because ROS 2 Humble is designed for Ubuntu 22.04 (Jammy). **VMware Workstation** was used to run Ubuntu as a virtual machine without modifying the main Windows installation.

## 2. Software and Requirements

| Requirement | Description |
| --- | --- |
| VMware Workstation | Installed on the Windows host computer |
| Ubuntu 22.04.5 Desktop ISO | 64-bit image |
| Internet connection | Required for Ubuntu package downloads |
| Storage space | Approximately 3 GB or more of additional free space for ROS 2 Humble Desktop packages |

## 3. Downloading Ubuntu

The Ubuntu 22.04.5 Desktop 64-bit ISO file was downloaded first. The downloaded file was then used as the installation image inside VMware Workstation.

![Figure 1. Ubuntu 22.04.5 Desktop ISO downloaded and ready for VMware](images/img-000.png)

## 4. Creating the Ubuntu Virtual Machine

A new virtual machine was created in VMware using the **Typical (recommended)** configuration.

![Figure 2. VMware Workstation before creating the new virtual machine](images/img-001.png)

![Figure 3. New Virtual Machine Wizard with Typical configuration selected](images/img-002.png)

### 4.1 Selecting the Ubuntu ISO

The downloaded Ubuntu ISO was selected as the installer disc image. VMware detected Ubuntu 64-bit 22.04.5 and prepared the installation.

![Figure 4. Ubuntu 22.04.5 ISO selected in VMware](images/img-003.png)

## 5. Installing Ubuntu 22.04.5 LTS

During the Ubuntu installer, English (US) was kept as the keyboard layout. A normal installation was selected, and the virtual disk was used for Ubuntu.

![Figure 5. Ubuntu keyboard layout selection](images/img-004.png)

![Figure 6. Ubuntu installation type and software options](images/img-005.png)

> The **Erase disk and install Ubuntu** option applied only to the virtual disk assigned to this VMware virtual machine. It did not erase the Windows host drive.

![Figure 8. Confirmation to write the partition changes to the virtual disk](images/img-006.png)

### 5.1 Creating the Ubuntu User

A local Ubuntu account was created. The username was set to `ubuntu`, and password-protected login was enabled.

![Figure 9. Ubuntu user account configuration](images/img-007.png)

### 5.2 Completing the Installation

Ubuntu copied the required files and completed the installation. The virtual machine was then restarted.

![Figure 10. Ubuntu installation in progress](images/img-008.png)

![Figure 11. Ubuntu installation completed successfully](images/img-009.png)

## 6. Preparing Ubuntu for ROS 2

After Ubuntu started, the terminal was used for all ROS installation commands. The computer hostname was changed to `ubuntu` so that the terminal prompt became easier to read:

```bash
sudo hostnamectl set-hostname ubuntu
```

![Figure 12. Hostname changed successfully to ubuntu](images/img-010.png)

### 6.1 Verifying the Ubuntu Version

```bash
lsb_release -a
```

The result confirmed **Ubuntu 22.04.5 LTS** with the Jammy codename.

![Figure 13. Verification of Ubuntu 22.04.5 LTS (Jammy)](images/img-011.png)

### 6.2 Updating the Package List

```bash
sudo apt update
```

The package index was updated before installing ROS and its dependencies.

![Figure 14. Ubuntu package list updated successfully](images/img-012.png)

### 6.3 Checking the Locale

```bash
locale
```

The system had UTF-8 locale support, including `LANG=en_US.UTF-8`, which is suitable for ROS 2.

![Figure 15. Locale configuration checked before ROS installation](images/img-013.png)

## 7. Preparing ROS 2 Repositories

### 7.1 Installing Repository Tools

```bash
sudo apt install software-properties-common
```

The package was already installed and up to date.

![Figure 16. software-properties-common checked successfully](images/img-014.png)

### 7.2 Enabling the Universe Repository

```bash
sudo add-apt-repository universe
```

The Universe repository was enabled and the package lists were refreshed.

![Figure 17. Universe repository enabled](images/img-015.png)

### 7.3 Installing curl

```bash
sudo apt install curl -y
```

curl was installed to download the ROS repository signing key.

![Figure 18. curl installed successfully](images/img-016.png)

### 7.4 Adding the ROS Repository Key

```bash
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```

The ROS signing key was saved in the Ubuntu keyrings directory.

![Figure 19. ROS repository signing key added](images/img-017.png)

### 7.5 Adding the ROS 2 Package Repository

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

![Figure 20. ROS 2 repository configuration command executed](images/img-018.png)

### 7.6 Updating After Adding ROS

```bash
sudo apt update
```

The output showed `packages.ros.org/ros2/ubuntu` for Jammy, confirming that the ROS repository was recognized correctly.

![Figure 21. ROS 2 package repository detected successfully](images/img-019.png)

## 8. Installing ROS 2 Humble Desktop

```bash
sudo apt install ros-humble-desktop
```

The installer reported that more than one thousand new packages were required and approximately **2.95 GB** of additional disk space would be used. The installation was confirmed by entering `Y`.

![Figure 22. ROS 2 Humble Desktop installation confirmation](images/img-020.png)

After the packages finished installing, the terminal returned to the command prompt without errors.

![Figure 23. ROS 2 Humble Desktop installation completed](images/img-021.png)

## 9. Configuring and Running ROS 2 Humble

The ROS environment was loaded into the current terminal session with:

```bash
source /opt/ros/humble/setup.bash
```

![Figure 24. ROS 2 Humble environment sourced successfully](images/img-022.png)

To load ROS automatically whenever a new terminal is opened, the setup command was added to `.bashrc` and the file was reloaded:

```bash
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

## 10. Final Verification

The ROS distribution environment variable was checked using:

```bash
echo $ROS_DISTRO
```

The terminal returned:

```
humble
```

This result confirms that ROS 2 Humble is installed, configured, and active.

![Figure 25. Final verification: ROS_DISTRO returns 'humble'](images/img-023.png)

## 11. Problems Encountered and Solutions

| Problem | Cause | Solution |
| --- | --- | --- |
| Username validation error during VMware Easy Install | The entered username contained unsupported formatting or characters | A simple lowercase username was used: `ubuntu` |
| Terminal prompt showed `ubuntu@ubuntu-virtual-machine` | VMware generated a long default hostname | The hostname was changed using: `sudo hostnamectl set-hostname ubuntu` |
| Mistyped `add-apt-repository` command | The command was entered incorrectly in the first attempts | The correct command `sudo add-apt-repository universe` was entered and completed successfully |
| `software-properties-common` did not install new files | The package was already installed | No action was required because Ubuntu reported that the newest version was already installed |
| ROS setup commands produced no visible output | `source`, echo-to-file, and some curl/tee commands can complete silently | The next verification commands and the absence of errors were used to confirm success |
| Large ROS installation size | ROS 2 Humble Desktop includes many tools and dependencies | The required disk usage was reviewed before entering `Y` to continue |

## 12. Final Result

**Ubuntu 22.04.5 LTS** was successfully installed inside VMware Workstation. **ROS 2 Humble Desktop** was then installed from the ROS package repository and configured to load automatically. The final command `echo $ROS_DISTRO` returned `humble`, confirming that the task was completed successfully.

## 13. Commands Used

```bash
lsb_release -a
sudo hostnamectl set-hostname ubuntu
sudo apt update
locale
sudo apt install software-properties-common
sudo add-apt-repository universe
sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
sudo apt update
sudo apt install ros-humble-desktop
source /opt/ros/humble/setup.bash
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
echo $ROS_DISTRO
```

## 14. Conclusion

The task demonstrated the complete process of preparing a Linux environment for robotics development. Ubuntu 22.04.5 LTS was installed safely in a VMware virtual machine, the required Ubuntu repositories were prepared, ROS 2 Humble Desktop was installed, and the ROS environment was configured. The successful `humble` output provides the final evidence that ROS 2 is ready for use.

---

