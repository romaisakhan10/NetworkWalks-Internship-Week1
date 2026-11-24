# Week 1 – Practical Lab Environment Setup (VirtualBox + Kali Linux)

**Internship:** Cybersecurity Internship, Networkwalks Academy
**Mentor:** Waqas Karim (CCIE)
**Intern:** Romaisa Khan
**Project:** WK1-PM1 – Lab Setup

## Objective
Build an isolated cybersecurity practice lab on my own laptop using VirtualBox and Kali Linux, so I can safely practice ethical hacking in later weeks.

## Requirements and Status
| Requirement | Status |
|---|---|
| VirtualBox (latest version) | Done – VirtualBox 7.2.22 |
| Kali Linux (latest) as VM | Done – Kali Linux 2026.3 (VirtualBox image) |
| NATNetwork with 10.0.0.0/24 | Done |
| Clipboard and drag/drop enabled | Done – Bidirectional |
| Shared folder (Downloads) from host | Done – mounted as `sf_Downloads` |
| Kali IP = 10.0.0.2/24 | [ ] confirm with `ip a` |
| Full internet access | Done – ping to google.com, 0% packet loss |
| Snapshot of the VM | [ ] confirm |

## Steps I Followed
1. Downloaded the Kali VirtualBox image from kali.org and verified its **SHA256** checksum with `Get-FileHash` (hash matched).
2. Installed 7-Zip/WinRAR and VirtualBox.
3. Extracted the Kali image to my D: drive (C: was full).
4. Created a NAT Network named `NatNetwork` with prefix `10.0.0.0/24` and DHCP enabled.
5. Imported the Kali VM and attached Adapter 1 to **NAT Network → NatNetwork** (Promiscuous Mode: Allow All).
6. Enabled **Shared Clipboard** and **Drag'n'Drop** as Bidirectional.
7. Added the Windows Downloads folder as a shared folder and added my user to the `vboxsf` group.
8. Configured the static IP `10.0.0.2/24`, gateway `10.0.0.1`, DNS `8.8.8.8`.
9. Tested connectivity with `ip a` and `ping -c 4 google.com`.
10. Took a snapshot of the clean VM.

## Screenshots
| Screenshot | File |
|---|---|
| Kali desktop running in VirtualBox | `screenshots/01-kali-desktop.png` |
| NAT Network (10.0.0.0/24, DHCP on) | `screenshots/02-nat-network.png` |
| VM network settings (NAT Network) | `screenshots/03-vm-network.png` |
| Clipboard and drag/drop (Bidirectional) | `screenshots/04-clipboard-dragdrop.png` |
| Shared folder list | `screenshots/05-shared-folder.png` |
| Shared folder visible in Kali | `screenshots/06-sf-downloads.png` |


## Problems I Faced and How I Solved Them
- **Download blocked (`ERR_CONNECTION_RESET`):** my ISP was resetting the connection to the Kali download. Solved by [write what worked: VPN / torrent / different network].
- **C: drive full (0 bytes free):** extraction failed with a write error. Solved by extracting the VM to the D: drive and setting VirtualBox's default machine folder to D:.
- **"Same UUID" error when importing:** an older Kali entry was already registered. Removed it with *Remove Only* and re-added the VM from D:.
- **Static IP not connecting:** the connection stayed stuck while Kali checked for duplicate addresses. Fixed with:
  ```
  sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
  sudo nmcli connection down "Wired connection 1"
  sudo nmcli connection up "Wired connection 1"
  ```

## What I Learned
- Why a NAT Network keeps lab traffic isolated from my real network.
- Why snapshots matter: I can roll back a broken VM in seconds.
- How to verify downloaded files with SHA256 checksums.
- Troubleshooting Linux networking with `ip a`, `ping`, and `nmcli`.

## Tools Used
VirtualBox, Kali Linux 2026.3, WinRAR/7-Zip, PowerShell, NetworkManager (`nmcli`)
