# Installing ONEOFFICEAUTOMATION

## 1. Requirements

| Item | Needed |
| --- | --- |
| Operating system | Windows 10 or 11, or Windows Server 2019 or later (64-bit) |
| Java | Java 21 or later (for example Eclipse Temurin 21 or Microsoft Build of OpenJDK 21) |
| Server | 4 cores, 8 GB RAM, 20 GB free disk for an evaluation; more for production |
| Network | Port 8720 on the server (the browser and runners connect to it) |
| Database (production) | Microsoft SQL Server 2019 or later; for an evaluation the built-in database is enough |
| Browser | Current Microsoft Edge or Google Chrome |

## 2. Install and first start

1. Download `ooa-customer-<version>.zip` from Releases.
2. Unzip it to a folder of your choice, for example `C:\OOA`.
3. Double-click `START-OOA.cmd`. It starts the Command Center, Agent Studio and a runner for this computer, then opens
   http://localhost:8720. Close the window (or press Ctrl+C) to stop everything.
4. Sign in as `admin` with the one-time password shown in the window. You are asked to choose your own password
   (at least 12 characters, with letters and digits).

To use another port: `START-OOA.cmd --port 8730`. The data is kept in `%USERPROFILE%\OOA Command Center`.

## 3. License

A licensed installation runs on **Microsoft SQL Server**. The evaluation can use the built-in database, but before you
install your license, set up SQL Server as in section 5. That moves your evaluation's data across, and until it is done
the Licenses page shows these steps in place of the install form.

1. Sign in as an administrator and open **Administration > Licenses**.
2. Choose your `.lic` file (or paste its text) and click **Install license**.
3. Give seats on the same page:

| Seat | Given to | Allows |
| --- | --- | --- |
| Developer | A person | Building and checking in agents |
| Citizen Developer | A person | Building and checking in agents |
| Attended Runner | A person | Running agents on their own runner |
| Unattended Runner | A runner | Scheduled, triggered, API and work-queue runs |

Before a license is installed, the product runs as a **30-day evaluation**. It allows 2 developers, 2 attended runners
and 2 unattended runners, given out automatically on first use. Administrators see a banner from 30 days before the
license expires. After the expiry date there are 14 grace days; then runs stop until a new license is installed.

To check a license file without installing it: `bin\ooa.bat license show yourfile.lic`.

## 4. Runners on other machines

1. In the Command Center: **Runners > Register a runner**. Copy the one-time key (valid for 24 hours).
2. On the runner machine, unzip the same release and run:

       bin\ooa.bat agent register --url http://YOUR-SERVER:8720 --key ooareg_...
       bin\ooa.bat agent start

3. In **Administration > Licenses**, give the runner an Unattended Runner seat.

## 5. Production database: Microsoft SQL Server (needed before licensing)

A database administrator creates two logins:

```sql
CREATE LOGIN ooa_setup WITH PASSWORD = N'...', CHECK_POLICY = ON;
ALTER SERVER ROLE dbcreator ADD MEMBER ooa_setup;
CREATE LOGIN ooa_app WITH PASSWORD = N'...', CHECK_POLICY = ON;
```

`ooa_setup` creates and upgrades the database. `ooa_app` is what the product runs with, and it only reads and writes
data. Then, with the product stopped, in the install folder:

```
bin\ooa.bat db configure --engine sqlserver --host DBSERVER --port 1433 --database ooa_command_center --user ooa_app --setup-user ooa_setup --tls required --tls-ca C:\certs\db-ca.crt
bin\ooa.bat db set-password
bin\ooa.bat db set-password --setup
bin\ooa.bat db init
bin\ooa.bat db test
```

To move an evaluation's data into SQL Server, add `bin\ooa.bat db copy --from "%USERPROFILE%\OOA Command Center"`
after `db init`. Passwords are stored encrypted with the vault key (`master.key` in the data folder, or the
`OOA_MASTER_KEY` environment variable). Keep that key with your backups.

## 6. Upgrading to a new release

1. Stop the product, and back up the data folder and the database.
2. Unzip the new release next to the old one, and copy across nothing but your start shortcut.
3. With SQL Server, run `bin\ooa.bat db migrate` once; the built-in database upgrades itself.
4. Start the new release with `START-OOA.cmd`.

## 7. Backup

Back up these together:

- the database (SQL Server: your usual full, differential and log backups; built-in: the data folder);
- the data folder's `config` and `repo-files` folders;
- the vault key (`master.key` or `OOA_MASTER_KEY`).

## 8. Health and support

- `http://YOUR-SERVER:8720/api/health/ready` reports whether the database, schema, vault key and disk space are in
  order (for monitoring tools).
- Forgotten administrator password: stop the product, run `bin\ooa.bat reset-admin`, then start it again and use the
  one-time password it prints.
