# Ardoise release notes

What changed in each version, newest first. Download the latest version at https://download.neolabs.cloud/ardoise

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
