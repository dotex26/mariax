# MariaX

**A portable local development stack for Windows.** One executable that
downloads, configures and supervises Nginx, PHP, MariaDB, PostgreSQL and
Redis — with phpMyAdmin, Adminer and phpRedisAdmin for each.

No installer. No Windows services. No registry pollution. Copy the folder
anywhere and it works; delete the folder and it's gone.

## Download

Get the latest build from **[Releases](../../releases/latest)**.

| File | Use this if |
|---|---|
| `MariaX.exe` | You just want the app. Put it in an empty folder and run it. |
| `MariaX-<version>-windows-x64.zip` | You also want the README and licence. |
| `mariaxctl.exe` | You want the headless command line as well. |

MariaX is a single self-contained executable, so `MariaX.exe` is a complete
install. **Put it in its own folder before running** — it creates `bin\`,
`data\`, `www\` and the rest beside itself.

Verify a download against `SHA256SUMS.txt`:

```powershell
Get-FileHash MariaX.exe -Algorithm SHA256
```

**Requires** Windows 10 1809 or later (64-bit) and the Microsoft Edge WebView2
Runtime, which ships with Windows 11 and current Windows 10.

## What it does

- **Five services, one button.** All start in parallel — measured at 665 ms.
- **Per-project PHP versions.** Each installed version runs its own pool of
  workers behind an Nginx upstream, so two projects can use PHP 8.1 and 8.3
  at once.
- **Local domains.** Add `myproject.test` and MariaX writes the server block
  and the hosts entry behind a single permission prompt, touching only its own
  marked block in that file.
- **A web console for every database**, all restricted to loopback so nobody
  on your network can reach your data.
- **Live logs** streamed into the dashboard rather than left in files.

## Why not XAMPP or Laragon

- **Nothing is left running.** Every service is held in a Windows Job Object,
  so closing MariaX — even killing it from Task Manager — takes all five down
  with it. No stray `mysqld.exe` holding a lock on its data directory.
- **Genuinely portable.** Configuration is regenerated from the current
  location on each launch, so moving the folder to another drive needs no
  edits. Paths containing spaces work, which breaks most local stacks.
- **Clean uninstall.** Delete the folder. That is the whole procedure.

## Customising

Files in `system\` are regenerated on every launch, so edits there would be
lost. The **Config** tab edits files under `system\custom\` that MariaX never
overwrites and that each service loads last, so your settings win. There is
also a checkbox list for PHP extensions.

## Command line

`mariaxctl.exe` is the same engine without the dashboard:

```
mariaxctl components   list installable servers
mariaxctl install      download what the enabled services need
mariaxctl run          start everything and stay in the foreground
mariaxctl ports        check for port conflicts
mariaxctl smoke        end-to-end self test
```

## Limitations

- Windows only.
- No HTTPS yet; a local certificate authority for `https://*.test` is planned.
- Nginx only — Apache is not bundled.
- Redis publishes no official Windows build, so MariaX uses the community
  `tporadowski` port, the same one Laragon uses.

---

MariaX does not bundle the servers it manages; it downloads them from their
vendors on first run, verifying checksums where published. Each remains under
its own licence — see `LICENSE.txt`.
