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

