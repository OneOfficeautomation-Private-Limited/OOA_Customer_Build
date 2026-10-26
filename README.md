# ONEOFFICEAUTOMATION: customer releases

This repository holds the installable ONEOFFICEAUTOMATION product for customers. Development happens in the
`oneofficeautomation` repository; only finished customer builds are published here.

## Download

Every release is under [Releases](../../releases). Each release has one file to download:

| File | What it is |
| --- | --- |
| `ooa-customer-<version>.zip` | The product: One Command Center, One Agent Studio and One Runner, with `START-OOA.cmd`, `README.txt` and the browser extension |

Your license file (`.lic`) is sent to you separately by ONEOFFICEAUTOMATION. It is never published here.

## Install in short

1. Install **Java 21** or later.
2. Unzip `ooa-customer-<version>.zip`, for example to `C:\OOA`.
3. Double-click **`START-OOA.cmd`**. Your browser opens http://localhost:8720.
4. Sign in as `admin` with the one-time password shown in the window, then choose your own password.
5. Open **Administration > Licenses**, install your `.lic` file, and give seats to your people and runners.

Without a license the product runs as a 30-day evaluation (2 developers, 2 attended runners, 2 unattended runners).

The full guide, covering runners on other machines, SQL Server, upgrades and backup, is in [INSTALL.md](INSTALL.md).
Changes per release are in [RELEASES.md](RELEASES.md).
