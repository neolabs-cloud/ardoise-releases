# Ardoise release notes

What changed in each version, newest first. Download the latest version at https://download.neolabs.cloud/ardoise

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



## Ardoise 0.9.11

**Update (keeps your VMs):** Host › Manage › Updates › Update now, on every host of a cluster.

### Changed
- **Ardoise Cluster now comes with Professional too.** Professional allows up to 3 hosts in a cluster; Enterprise has no limit on hosts in a cluster. The limit is per cluster, not on how many hosts you license. A cluster that has a Professional host follows the Professional limit, so for more than 3 hosts every host in it needs Enterprise. Community hosts can't join a cluster.


