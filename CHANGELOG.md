# Ardoise release notes

What changed in each version, newest first. Download the latest version at https://download.neolabs.cloud/ardoise

## Ardoise 0.9.21

**Update (keeps your VMs):** Cluster › Updates › Update all hosts, or Host › Manage › Updates on each host.

### New
- **Backups follow a moved VM.** When a VM moves to another host (by hand, in maintenance mode or during Update all hosts), it leaves its backup jobs on the old host and joins the job of the same name on the new host, which is made with the same schedule, number kept and datastore when it isn't there yet. Backups made so far stay where they were and can still be restored there. The move's event says what happened.
- **Shared datastores for backups.** A new backup job in a cluster lists the datastores every host mounts first, marked "every host", and picks one by default, so backups stay in one place wherever the VM runs.

### Fixed
- **Consoles of VMs on another host** of the cluster opened and closed at once ("This console is closed"). The screen and the text console now open from any host.
- **Switching hosts in the navigator:** clicking another host while on a Host page now opens it, without going through the cluster first.
- **Update all hosts** found no update until the host checked by itself once a day: Cluster › Updates checks when it opens and has a Check for updates button.
- **A backup of a VM that moved away** says which host it runs on now, instead of "VM not found".


## Ardoise 0.9.20

**Update (keeps your VMs):** Host › Manage › Updates › Update now on each host, or Cluster › Updates › Update all hosts (every host needs 0.9.19 or later for that).

### New
- **Time zone help.** When a host's time zone isn't your browser's, the Host page says so, with one button to use your browser's zone (nothing changes until you press it). Cluster › Settings › Time zone shows every host's zone and sets one zone on all of them; each host keeps its own time servers.
- **Setup checks licensing again by itself.** Right after a host starts, its network may not be up yet: the Welcome screen now checks again every few seconds and turns on "Get a free Community key" as soon as licensing answers. It says why it can't reach licensing, and has Try again.
- **Browser tabs name the host** ("ardoise-02 · Ardoise"), so several hosts are easy to tell apart.

### Better
- **Moving VMs:** the progress shows the disk copy and the memory copy apart, in GB with the speed, and the VM list shows the VM on its new host as soon as the move ends.
- **Update all hosts** checks first that every host can move VMs (0.9.19 or later) and says which one to update by itself, instead of stopping halfway.
- **After a drop to Community**, VMs keep running and can still be edited and started; only adding more vCPUs than Community allows is refused.
- **Creating a cluster:** if creating fails, try again without adding the hosts back. The version check only ever suggests updating the older host.
- **Installer:** a disk from an earlier install (Proxmox and other LVM or RAID setups) is released before it is erased, so the install no longer stops at "partprobe".
- **Tailscale setup:** the connect dialog asks for an OAuth client with only the tag tag:ardoise, and the policy rules it shows let that tag own tag:ardoise-migrate, so Tailscale makes both kinds of keys.

### Fixed
- The user menu names the host you signed in on, also while you look at another host of the cluster.
- The Ardoise Converter's source choice (this network or over Tailscale) shows which one is picked.
- Tenant hosting on a not-for-resale or trial key says it is not billed.
- The Tenants page says "one host" on a host that is not in a cluster.
- The setup's "Set a fixed address later" shows only when the address comes from DHCP.


## Ardoise 0.9.19

**Update (keeps your VMs):** Host › Manage › Updates › Update now on each host, or Cluster › Updates › Update all hosts.

### Fixed
- **Moving VMs between cluster hosts.** Every move to another host stopped at "Connecting to the other host" with "can't reach the other host's hypervisor". Moves now connect, live or powered off, with or without copying the disks.


## Ardoise 0.9.18

**Update (keeps your VMs):** Host › Manage › Updates › Update now on each host, or Cluster › Updates › Update all hosts. The update installs Tailscale when the host can reach the internet; it stays off until you set it up.

### New
- **Ardoise Converter: Windows or Linux machines into VMs.** Migration › New migration › Windows or Linux machine copies a running server, desktop or laptop (physical, or a VM on any other hypervisor or cloud) into a new VM on this host. Ardoise gives you one command to paste on the machine: PowerShell for Windows (ScreenConnect backstage, a remote console or an RMM script) or a shell command for Linux. Nothing is installed in Windows and nothing needs signing. Windows is copied live from a Windows shadow copy, so users keep working and files, SQL and Active Directory stay consistent; Linux is copied live with no snapshot, so stop databases and busy services first. Only used space is sent, compressed, and the copy picks up where it stopped when the connection drops. Then the usual guest preparation adds the VM drivers, the VM can keep the machine's IP address and MAC address, and the boot check starts it with its network unplugged. Windows Server 2012 R2 and newer, Windows 10 and 11; Ubuntu, Debian, Red Hat, Rocky and Alma with Python 3 and lsblk 2.27 or newer (not CentOS or RHEL 7). If the Ardoise service restarts during a copy, the machine sends again what was lost and carries on. Cancel copy on the Migration page stops a copy.
- **Migrate from other locations over Tailscale.** Networking › Tailscale puts the host on your own tailnet (free or paid plan) with an OAuth client or auth keys, so you can also manage it from anywhere. A migration from another location installs Tailscale on the machine (Windows 10 or Server 2016 and newer) with a one-time key; with the policy rules Ardoise shows, it reaches only this host's migration port, shows the upload speed and the time it will take, can hold back during the site's business hours, and removes Tailscale when the copy ends (a Tailscale the machine already had stays).

### Fixed
- **Updates card** no longer says Up to date right after a restart or when a check failed: it keeps what the last check found, and says Not checked yet or Couldn't check.
- **Upload percent** in the Datastore browser matches Recent tasks.
- **Greyed-out actions say why**, for example "Shut down the VM to delete it" or "Administrators only".
- **Host page** shows the ISO library by its datastore name.
- **Sign-in for non-administrators** without a license no longer links to the website; it says to ask an administrator.


## Ardoise 0.9.17

**Update (keeps your VMs):** Host › Manage › Updates › Update now on each host, or Cluster › Updates › Update all hosts.

### New
- **Protect console for Linux VMs with a graphics card.** With a graphics card passed through (an integrated one above all), Linux's screen driver can slow to a crawl and hold the console while it does. Services that use the console, SSH among them, then wait forever. A Linux VM with a graphics card now shows Protect console on its Summary. Through the guest tools, Ardoise switches the guest to Linux's simple built-in screen driver and turns on the Text console login prompt, then restarts the guest if you want. The graphics card itself isn't touched. Undo it from Actions.
- **The Text console login prompt turns on by itself.** Linux looked for consoles before the text console showed up, so it could stay blank. Once the guest tools answer, Ardoise turns the login prompt on in the guest, one time, and it stays on through restarts. Edit settings › VM Options › Console › Login prompt turns this off. If the Text console is still blank, a bar at the top offers Turn on the login prompt. The Linux guest tools disc does it too.

### Fixed
- **Shut down and Restart guest OS no longer hang the VM page.** Ardoise used to wait on the guest tools while they were busy shutting the guest down, so the page stayed on Loading. Now the request returns at once, and anything the guest tools can't answer within a few seconds shows as Busy. The page says Shutting down, and if the guest is still running after 5 minutes it offers Power off now. It never powers off by itself.


## Ardoise 0.9.16

**Update (keeps your VMs):** Host › Manage › Updates › Update now on each host, or (new) Cluster › Updates › Update all hosts. Linux VMs get the text console from their next power off and on.

### New
- **Update a whole cluster with one button.** Cluster › Updates lists every host's version and what happens to its VMs. Update all hosts does them one at a time: the host's VMs move to the others, it updates and restarts if needed, comes back healthy, and its VMs move back. You choose the order; the host you are signed in on goes last. VMs that can't move (local disks, PCI devices) shut down and start again after, or that host is skipped, as you choose. Stop after this host ends it cleanly, and the job carries on by itself after this host's own update.
- **Text console.** A plain terminal next to the screen in the VM console (Screen and Text at the top). It doesn't use the video card, so it keeps working when the screen can't, such as a VM with a graphics card passed through. Linux shows a login prompt on it by itself. It's on for every Linux VM from its next power on; Edit settings › VM Options › Console turns it off. Paste text, Send keys and the keyboard work in both.
- **Guest tools for Linux without internet.** Install guest tools on a Linux VM puts a guest tools disc in its CD/DVD drive with the guest agent for Ubuntu 22.04 to 26.04, Debian 12 and 13, and Red Hat, Rocky and Alma 8 to 10. Copy the one command, or press Type it in the console. The network can stay unplugged. Ardoise ejects the disc once the agent answers.
- **Windows setup answered when deploying from a template.** For Sysprep templates, Deploy VM can set the computer name (the VM's name), time zone, Administrator password, product key and join a domain. The answers ride on a small disc only that VM sees; Ardoise removes it once the VM is up, and the passwords are not kept.
- **Configuration copies to cloud storage.** The nightly configuration backup can go to cloud storage that speaks the S3 protocol, from any provider, so a host that is lost can be rebuilt from a copy kept elsewhere. Test connection checks the key first. Copies are encrypted with the backup passphrase before they leave the host, and the key is kept encrypted on the host.
- **Firmware ROM file for PCI devices.** The few graphics cards that only start in a VM with their own ROM file can use one from a datastore (Edit settings › PCI device › Firmware ROM).

### Fixed
- **Edit settings follows the VM.** It no longer says Running for a VM that was shut down while the window was open (or the other way round).
- **Cluster actions for another host reach that host.** Entering or leaving maintenance mode, moving VMs and starting VMs elsewhere from the cluster page now always act on the host you picked.
- **A VM with a graphics card passed through** shows a note on its Summary: the screen can stop updating, and the Text console always works.


## Ardoise 0.9.15

**Update (keeps your VMs):** Host › Manage › Updates › Update now, on every host of a cluster.

### New
- **Send keys** in the VM console: the shortcuts your computer keeps for itself, sent to the VM in one click. Windows: Ctrl+Alt+Del, Windows key, Alt+Tab, Alt+F4. Linux: Ctrl+Alt+F1 (login screen), Ctrl+Alt+F2, Ctrl+Alt+F7 (back to the desktop), Ctrl+C. Send Ctrl+Alt+Del moved into this menu.
- **On-screen keyboard** in the VM console: a full keyboard under the screen; click any key. Ctrl, Alt, Shift and Win stay held until the next key, so Ctrl, Alt, F1 sends Ctrl+Alt+F1. On a phone the keys are sized to fit.

## Ardoise 0.9.14

**Update (keeps your VMs):** Host › Manage › Updates › Update now, on every host of a cluster.

### Fixed
- **Unplugged network cards now really stay unplugged.** A VM started with its network unplugged (a migration's boot check, or Connected unticked while it was off) could still send and receive traffic. Ardoise now cuts the cable in the running VM itself on every start, plug-in and unplug, checks it again every 5 seconds, and stops a VM that should be unplugged when it can't be. Ticking Connected brings the network back at once.
- **Adding drivers during a migration or import works again.** It stopped with "not a datastore folder", so migrated VMs kept SATA disks and E1000e network cards. A problem like this now shows on the migration's row and in its boot check, not only in the log.
- **Boot checks are honest about Linux.** When the guest agent never answers and only the screen stopped changing, the check says Likely started (not Passed) and asks you to look at the console.
- **Migration sizes** show what the disks really hold (the source often reports a few KB for a VM that is off), and the list shows datastores by their name.
- **The VM page** picks up a VM that was shut down from inside within 5 seconds.
- **Nothing cut off:** long disk models, VM names, guest OS names and notes in Pick VMs wrap instead of ending in "...". Network ports used by a virtual switch read the same on every row of PCI devices.

### New
- **Paste text in the console.** Paste text types what you paste into the VM, one key at a time, for VMs without a shared clipboard.

## Ardoise 0.9.13

**Update (keeps your VMs):** Host › Manage › Updates › Update now, on every host of a cluster.

### New
- **PCI passthrough.** Give a VM a real device of the host: a graphics card or the processor's built-in graphics (hardware video encoding for Plex or Jellyfin), a network port, a USB controller, a Wi-Fi card, a disk controller. Host › Manage › Hardware › PCI devices lists every device with what it is, whether it is passed through, and which VMs use it.
- **Turn on passthrough once per host.** Ardoise turns on the processor's device isolation (Intel VT-d or AMD-Vi), which needs one restart: now, or at the next restart. If the firmware has it off, the page says so and what to turn on in the BIOS or UEFI setup.
- **Toggle passthrough without a restart.** Select a device and press Toggle passthrough: the host lets go of it in a few seconds. A device whose driver can't let go while the host runs switches over at the next restart, and the page offers Restart host. Devices that must move together (an isolation group) are shown before you confirm, and the dialog warns when the host's own screen goes blank.
- **Devices Ardoise needs are locked:** the system disk, datastore disks and the network ports of virtual switches, including the management network. Each shows why. Chipset parts that can't be passed through are under Show all devices.
- **Add PCI device** in a VM's Edit settings (VM powered off), from the devices switched on for passthrough. It shows under Devices and on the VM's Summary.
- **Rules that keep it safe:** a device belongs to one running VM at a time (a second VM is refused at power on, with the name of the one using it); a VM with a device stays on its host (moves and maintenance mode shut it down instead) and takes snapshots without memory; a device is never taken from a running VM; only host administrators can change PCI devices, and tenant users never see them.

## Ardoise 0.9.12

**Update (keeps your VMs):** Host › Manage › Updates › Update now, on every host of a cluster. Tenants and tenant users wait until every host of the cluster runs 0.9.12.

### New
- **Tenants, for MSPs** (Enterprise with the Tenant hosting add-on). One cluster, or one host, can now hold many client companies, each kept apart. A tenant has its own VMs, its own port group (VLAN) on every host, its own limits, its own users, its own backup job and a monthly usage report. Navigator › Tenants lists them with what each one uses.
- **New tenant** walks you through five steps: name and color, network, limits, users and backups, review. The color picker has a color area, hue, hex and RGB, an eyedropper and 24 quick picks, so a tenant can match the client's brand color; it warns when a color is close to another tenant's. Ardoise makes the tenant's port group on every host of the cluster, so its VMs can move between hosts.
- **Limits** for each tenant: vCPUs and memory of running VMs, and the full size of every disk, on or off, counted across every host. Creating, starting, growing or moving a VM into a tenant past a limit is refused with how much is left. A tenant can be limited to some datastores.
- **Move to a tenant** (Virtual Machines › Actions): puts VMs you have into a tenant, and can move their network adapters to the tenant's port group. VMs keep running. A Tenant column and a Viewing switch in the Navigator show one tenant's VMs at a time.
- **Tenant users.** Two roles: Tenant admin (consoles, power, snapshots, restore their own backups, create VMs within the limits, invite users) and Viewer (sees the VMs and their state). They are invited by email with a link that works once, for 72 hours; they choose their own password and set up two-step sign-in, which is always on for them. They sign in at the same address and see only their own company: never the hosts, other tenants, storage, the physical network, the license or updates.
- **Tenant backups.** Each tenant can have its own backup job; every host backs up the tenant's VMs it runs. Tenant admins see their backups, restore them and back up now.
- **Usage report** (Tenants › Usage report): averages per tenant for the month (vCPUs, memory, storage, backup space, VMs, share of time running), counted every hour on every host. Download it as a CSV for your billing system, and have it emailed on the 1st to you and, if you want, to each tenant's contact.
- **Tenant hosting on Licensing:** tenants in use of the tenants your license covers. Tenant hosting is sold by Neo Labs sales (hello@neolabs.cloud): billed per tenant, monthly, from 3 tenants. If it ends, existing tenants keep running; only new tenants can't be made.

### Fixed
- **Nothing is cut off on short screens.** On a laptop screen the page scrolls instead: hardware tables, migration sources and the last cards on a page show in full, and the VM list headers wrap.

## Ardoise 0.9.11

**Update (keeps your VMs):** Host › Manage › Updates › Update now, on every host of a cluster.

### Changed
- **Ardoise Cluster now comes with Professional too.** Professional allows up to 3 hosts in a cluster; Enterprise has no limit on hosts in a cluster. The limit is per cluster, not on how many hosts you license. A cluster that has a Professional host follows the Professional limit, so for more than 3 hosts every host in it needs Enterprise. Community hosts can't join a cluster.

## Ardoise 0.9.10

**Update (keeps your VMs):** Host › Manage › Updates › Update now. Update every host to 0.9.10 before you make a cluster: all hosts in a cluster run the same version.

### New (Enterprise)
- **Ardoise Cluster.** Manage several hosts from any one of them, with no management VM to install. Host › Ardoise Cluster › Create cluster names the cluster and adds the other hosts. Each host is checked before it joins: Enterprise license, same Ardoise version, time, networks and processor. VMs keep running on every host, nothing is erased, and a host belongs to one cluster at a time.
- **Pairing code to add a host.** On the host that joins: Host › Actions › Allow joining a cluster shows a code like 7KQ4-M2XD-P9WF. Type it, with that host's address, under Add host. The code works once, for 10 minutes, and no password leaves the host. You can also add a host with its root password and its two-step code; the password is used once and never stored.
- **Sign in on any host.** The Navigator lists the cluster, each host with its status, and Virtual Machines for all hosts. Open a host to manage it as before: storage, networking, backups, updates and the shell all work through the host you signed in on.
- **One set of users.** Users, roles and two-step sign-in are the same on every host. Add or change a user on any host and the others get it within seconds; a host that was off catches up when it is back.
- **Move VMs between hosts.** Tick VMs in Virtual Machines, then Move. A running VM moves with no downtime. Its disks stay where they are when they are on a datastore every host sees; otherwise they are copied to the other host while the VM keeps running. A VM that is off moves its files. Each move is checked first (memory, networks, datastore, processor, VM name) and shows in Recent tasks. Moves are encrypted by default (Cluster › Settings).
- **CPU level for VMs** (Cluster › Settings): when the hosts have different processors, every VM sees the same processor features on every host, so it can move while running. VMs pick it up at their next start.
- **Shared datastores.** Cluster › Actions › Add shared datastore mounts one NAS share (NFS) on every host at once, or on none if one host can't.
- **Maintenance mode for a host in a cluster.** Its running VMs move to the other hosts first, to the host with the most free memory unless you pick another. A VM that can't move is shut down and starts again when you exit maintenance.
- **A host that stops answering** shows as Not responding after 15 seconds, with an alert. Its VMs show as Unknown. Ardoise never starts them elsewhere by itself. Cluster › What to do starts VMs with disks on a shared datastore on another host, only when every other host agrees it is down, its run marks have stopped, and you type its name.
- **Recent tasks** shows the tasks of every host, with a Host column.

### Changed
- **Professional covers up to 3 hosts, each managed on its own.** Ardoise Cluster is in Enterprise, with no limit on hosts.

## Ardoise 0.9.9

**Update (keeps your VMs):** Host › Manage › Updates › Update now. The update installs the hardware sensor tools (ipmitool, lm-sensors, dmidecode), which needs internet access; without it, everything else installs and Monitor › Hardware shows disk health only until a later update.

### Changed for every edition
- **Alerts are now in every edition, Community included:** email, Microsoft Teams, Slack and the weekly health email. Before 0.9.9, email alerts needed Professional. SNMP and remote syslog stay in Professional and Enterprise.
- **Export is free in every edition.** A VM can always leave the host as an OVA or OVF, also on Community and after a license ends. Import and migration stay part of Professional.

### New
- **Two-step sign-in.** After the password, Ardoise asks for the 6-digit code from an authenticator app on your phone (any app that shows 6-digit codes). Set it up under your name › Settings › Account: scan the code or type the key, type the code it shows, then save the ten recovery codes, shown once; each signs in once if you lose your phone. "Don't ask again on this browser for 30 days" is a tick box at sign-in; it ends when two-step is reset or turned off, or the password changes. **Administrators need it by default:** an administrator without it is asked to set it up right after the password, nobody is locked out by the update. The same goes for scripts that sign in to the API: until two-step is set up for that account in a browser, they can read but not change anything. Host › Manage › Users shows who has it, the rules (administrators, or everyone) and Reset two-step for a lost phone; every reset is in Recent tasks and can alert. Wrong codes count toward the account lock. **Every administrator lost their phone?** At the host's console: System Customization › Reset Two-Step Sign-In (or `ardoised two-step-reset <user>` as root). Neo Labs cannot reset it for you.
- **Hardware health** (Host › Monitor › Hardware): power supplies, fans, temperatures, memory and disks in six tiles, with each disk's health, temperature, wear, hours on and reallocated sectors, and the sensors. Connect the server's management controller (iDRAC, iLO or any Redfish or IPMI controller, a read-only account is enough) with Test; its certificate is trusted only once you tick "Trust this certificate", and the password stays on the host. Without a controller Ardoise reads what the host itself sees. Hardware alerts (power or fan failure, temperature, memory errors, disk health) are on for every host. The Host page gets a Hardware tile, Licensing and Updates share one Connected services tile, and a hardware warning turns Normal amber.
- **Configuration backup** (Host › Manage › Configuration backup): one encrypted file with the network, firewall, datastores, users and roles with two-step sign-in, alerts, license, time, web certificate and every VM's settings (never the disks: Backups has those). Download one with your own passphrase. A copy is also saved every night at 2:00 AM (the last 14 are kept) to a NAS datastore when there is one; on a local datastore the page warns that a failed host takes the copy with it. **Restore from a file** shows what changes in each area and asks you to type the host name; the settings from before are saved first, VMs keep running and the services restart for about a minute. A file from another host keeps this host's name, addresses, certificate and license.
- **Alerts in Microsoft Teams and Slack**, next to email, for the same events; each channel can send everything, or only Critical and Warning. Paste the channel's webhook address and Send test; the address is kept on the host and never shown again in full.
- **Weekly health email**, every Monday at 7:00 AM by default: host, backups, disk space, updates, hardware, alerts and VMs, with a subject that says if something needs a look. Preview it with this week's data. Email server settings moved into Email › Settings.
- **Check that it starts** after a migration or an import (on by default, Professional and Enterprise): the copy starts once with its network unplugged, so it never meets the original on the network. Ardoise waits for the guest agent (up to 10 minutes), keeps a picture of the screen and shows the result in the Migration list. A copy that passed keeps running unplugged until you press Plug network in; one that failed keeps running so you can look at it, with the likely cause.
- **Passwords and keys kept on the host are encrypted.** Email passwords and API keys, migration source passwords, chat webhook addresses, the management controller password and the backup passphrase are encrypted with a key that never leaves the host. Ones saved before are encrypted by themselves on the first start after the update; nothing to do. (SMB datastore credentials stay in their owner-only file, which the system reads when it connects the share.)

### Fixed
- **Typing just the address works.** Opening `192.168.1.10` without `https://` used to sit on a blank page (Safari on iPhone tries plain `http://` first and waited until it gave up). Port 80 now sends the browser straight to the secure page, and closed ports answer "no" at once instead of making a device wait. Set `http_redirect_port = 0` in `/etc/ardoise/ardoised.toml` to turn the redirect off.
- **Host page charts** show their "Last hour / Peak" line again on a 1440 × 900 screen.
- **Email errors in plain words.** When the email service refuses an alert (a sender it does not allow, a wrong API key, an address on its do-not-send list, a rate limit or an outage), Monitor › Notifications says what happened and what to do, instead of showing the service's raw answer.

## Ardoise 0.9.8

**Update (keeps your VMs):** Host › Manage › Updates › Update now.

### New
- **Welcome window on a new host.** The first sign-in walks you through five steps: get a free Community key (enter your name, email and country; the key arrives by email) or paste a key you have, then set the time zone, time server and host name. Nothing to set up for email: Neo Labs licensing sends the key.

### Fixed
- **No more sign-out when the host is slow to answer.** If the host didn't answer the session check in time (for example while a NAS share was waking up), the portal signed you out. Now it waits and asks again; only the host can end a session.
- **A slow or cut-off answer no longer breaks the page** (the red "Cannot read properties of null" message): it is retried, and the page reconnects by itself once the host answers.
- **"Still there?" shows in the browser tab title too,** with the seconds left, so you see it while working in another tab.
- **Lost connections are recorded.** When the web client loses the host and gets it back, the host's log says which request failed, why, and for how long.
- **Updates page** shows a new version right after Help › Check for updates finds it (it used to say "Up to date" until you reloaded).
- **Host page** shows a datastore's new name after a rename.
- **Long install media names work.** Create VM refused an ISO with a name over 63 characters (Microsoft's Windows Server ISO, for one). File names in datastores can now be up to 200 characters.

### iPhone, iPad and Mac
- **No more zoomed, sliding pages on iPhone.** Text boxes are 16 pt, so Safari no longer zooms in when you tap one (it used to zoom in at sign-in and stay zoomed).
- **iPhone:** tabs are one row of pills you can swipe, with the open one in view; crowded toolbars keep what fits and put the rest under More; virtual machines and every table show as cards; windows open full screen with their buttons at the bottom; Recent tasks is a small button in the top bar with a running count.
- **Mac and laptops:** every page fits the window from 1280×720 up; long lists scroll inside their own card.
- **Datastore browser:** the upload progress sits in the bottom bar, so the page no longer scrolls while an ISO uploads.
