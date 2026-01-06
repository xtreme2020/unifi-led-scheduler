# UniFi LED Scheduler
**Lightweight Docker-based scheduler for UniFi AP LEDs (no Home Assistant required)**

Get your UniFi Access Point LEDs on a schedule in under **5 minutes** 🚀

---

## What This Does

Automatically turns **UniFi Access Point LEDs ON and OFF** based on a configurable schedule using the **UniFi Controller API**.

---

## Why This Exists

UniFi does **not** provide native LED scheduling.

This project is designed for users who want:

- ✅ A lightweight, single-purpose solution
- ✅ No Home Assistant dependency
- ✅ Docker / server / homelab–friendly automation

---
### Supported Devices and Controllers
Look here for compatibility info:
https://github.com/Art-of-WiFi/UniFi-API-client
---
## Prerequisites

- ✅ Docker and Docker Compose installed
- ✅ UniFi Controller running and accessible
- ✅ **Local admin account** on your UniFi Controller (see below)

## ⚠️ Important: Credential Requirements

**This script requires a local administrator account WITHOUT two-factor authentication (2FA).**

### Why Local Admin?

- API access doesn't work with Ubiquiti SSO accounts
- 2FA cannot be used for automated scripts
- Remote access is NOT required (safer!)

### How to Create a Safe Local Admin

**Recommended**: Create a dedicated local-only admin account for this script:

1. Log into your UniFi Controller
2. Go to **Settings** → **Admins & Users**
3. Create a new admin with **local access only** (no remote access)
4. **Do NOT enable 2FA** for this account
5. Use a strong password

**Step-by-step guide**: https://help.spotipo.com/en/articles/11623978-add-local-admin-to-unifi-controller

### Security Best Practices

✅ **Use a separate admin account** - Don't use your main admin account  
✅ **Disable remote access** - Local network only  
✅ **Strong password** - Use a long, unique password  
✅ **Limited to API access** - Only used by this script  
✅ **Monitor logs** - Check logs regularly for any issues  

## 🚀 Quick Start (4 Steps)

### Step 1: Configure Your Settings

Copy the example configuration file:

```bash
cp .env.example .env
```

Edit `.env` with your details:

```bash
nano .env
```

**Required settings:**

```bash
# Your UniFi Controller credentials
# IMPORTANT: Must be a LOCAL admin account WITHOUT 2FA
# See "Credential Requirements" section above
UNIFI_USER=your_local_admin_username
UNIFI_PASSWORD=your_password
UNIFI_URL=https://192.168.1.1:8443    # Your controller IP/URL

# When to turn LEDs on/off (24-hour format)
LED_ON_TIME=06:00:00     # 6 AM
LED_OFF_TIME=22:00:00    # 10 PM

# Which site(s) to control
UNIFI_SITES=default

# Which APs to control (leave empty for ALL APs)
AP_MACS=
```

Save and exit (`Ctrl+X`, then `Y`, then `Enter`).

### Step 2: Use the Included docker-compose.yml

**The repository includes a ready-to-use `docker-compose.yml` file!**

No need to create one - it's already configured with:
- ✅ Automatic image pulling from GitHub Container Registry
- ✅ Log rotation (1 MB limit, 3 files max)
- ✅ Proper volume mounts for scripts and logs
- ✅ Network configuration

Just use the included file as-is. Your timezone is controlled by the `TZ` variable in your `.env` file.

### Step 3: Start the Container

```bash
docker compose up -d
```

Docker will automatically pull the latest image from the registry.

### Step 4: Verify It's Working

```bash
# Check the logs
docker compose logs -f

# Run manually to test
docker compose exec unifi-led-scheduler php /app/toggle_ap_led.php
```

**That's it!** Your LEDs will now automatically toggle on schedule. 🎉

---

## 📋 Common Tasks

### View What's Happening

```bash
# Live container logs
docker compose logs -f

# Cron execution logs
docker compose exec unifi-led-scheduler tail -f /var/log/cron.log
```

### List Your Access Points

Find out which APs you have and their MAC addresses:

```bash
docker compose exec unifi-led-scheduler php /app/list_aps.php
```

Output example:
```
Site: default
-------------------------------------------
  Living Room AP          | MAC: aa:bb:cc:dd:ee:ff | Model: U6-Lite      | IP: 192.168.40.10    | LED: on     | Status: Online
  Office AP               | MAC: 11:22:33:44:55:66 | Model: U6-Pro       | IP: 192.168.40.11    | LED: on     | Status: Online
```

### Control Specific APs Only

Edit `.env` and add the MAC addresses:

```bash
# Control only these specific APs (comma-separated)
AP_MACS=aa:bb:cc:dd:ee:ff,11:22:33:44:55:66
```

Then restart:

```bash
docker compose restart
```

### Change LED Schedule Times

Edit `.env`:

```bash
LED_ON_TIME=07:00:00     # 7 AM
LED_OFF_TIME=23:00:00    # 11 PM
```

Restart:

```bash
docker compose restart
```

### Control Multiple Sites

Edit `.env`:

```bash
UNIFI_SITES=default,office,warehouse
```

Restart:

```bash
docker compose restart
```

### Stop/Start/Restart

```bash
docker compose down              # Stop
docker compose up -d            # Start
docker compose restart          # Restart
```

---

## 🔧 Configuration Examples

### Example 1: All APs, Default Schedule
```bash
UNIFI_SITES=default
AP_MACS=
LED_ON_TIME=06:00:00
LED_OFF_TIME=22:00:00
```

### Example 2: Specific APs Only
```bash
UNIFI_SITES=default
AP_MACS=aa:bb:cc:dd:ee:ff,11:22:33:44:55:66
LED_ON_TIME=06:00:00
LED_OFF_TIME=22:00:00
```

### Example 3: Multiple Sites
```bash
UNIFI_SITES=default,office,home
AP_MACS=
LED_ON_TIME=07:00:00
LED_OFF_TIME=23:30:00
```

### Example 4: Specific APs Across Multiple Sites
```bash
UNIFI_SITES=default,office
AP_MACS=aa:bb:cc:dd:ee:ff,11:22:33:44:55:66
LED_ON_TIME=06:00:00
LED_OFF_TIME=22:00:00
```

---

## 🐛 Troubleshooting

### Container won't start

```bash
docker compose logs
```

Check for errors in the output. Common issues:
- Wrong credentials in `.env`
- Using SSO/Cloud account instead of local admin
- 2FA enabled on the admin account
- UniFi Controller URL incorrect
- Network connectivity issues

### LEDs aren't changing

1. **Check if script is running:**
   ```bash
   docker compose exec unifi-led-scheduler php /app/toggle_ap_led.php
   ```

2. **Verify credentials:**
   - Make sure `UNIFI_USER`, `UNIFI_PASSWORD`, and `UNIFI_URL` are correct in `.env`

3. **Check if it's the right time:**
   - The script only changes LEDs during the scheduled times
   - Run manually to test: `docker compose exec unifi-led-scheduler php /app/toggle_ap_led.php`

### Wrong timezone

The container automatically syncs with your host system timezone. To override:

Edit `.env`:
```bash
TZ=America/New_York
```

Restart:
```bash
docker compose restart
```

### Can't reach UniFi Controller from container

If your Controller is on the same machine, try using host networking.

Edit `docker-compose.yml`:
```yaml
services:
  unifi-led-scheduler:
    network_mode: host
    # ... rest of config
```

Restart:
```bash
docker compose down
docker compose up -d
```

### No APs found

Check that:
1. Your site name is correct in `UNIFI_SITES`
2. You have access points in that site
3. Your credentials have permission to see devices

Run:
```bash
docker compose exec unifi-led-scheduler php /app/list_aps.php
```

---

## 📊 How the Schedule Works

The container runs a check **every hour** as a failsafe and
by default, the schedule is:
- **Between 10 PM and 6 AM**: Turns LEDs **OFF**
- **Between 6 AM and 10 PM**: Turns LEDs **ON**

The schedule also runs at the exact ON/OFF times you configure in `.env` with `LED_ON_TIME` and `LED_OFF_TIME`.

---

## 🔄 Updating

### Update Configuration

Edit `.env`, then:
```bash
docker compose restart
```

### Update to Latest Image

Pull the latest image version:
```bash
docker compose pull
docker compose up -d
```

---

## 🆘 Quick Reference Commands

```bash
# Start
docker compose up -d

# Stop
docker compose down

# Restart
docker compose restart

# View logs
docker compose logs -f

# List all APs
docker compose exec unifi-led-scheduler php /app/list_aps.php

# Run script manually
docker compose exec unifi-led-scheduler php /app/toggle_ap_led.php

# Check container time
docker compose exec unifi-led-scheduler date

# View cron logs
docker compose exec unifi-led-scheduler tail -f /var/log/cron.log
```

---

## 💡 Tips

1. **Test first**: Run the script manually before relying on the schedule
   ```bash
   docker compose exec unifi-led-scheduler php /app/toggle_ap_led.php
   ```

2. **Check your APs**: Use the list command to see all available APs
   ```bash
   docker compose exec unifi-led-scheduler php /app/list_aps.php
   ```

3. **Enable debug mode**: See detailed output by editing `.env`
   ```bash
   DEBUG_MODE=true
   ```
   Then restart: `docker compose restart`

4. **Monitor regularly**: Check logs occasionally to ensure everything is working
   ```bash
   docker compose logs --tail=100
   ```

5. **Backup your .env**: Your `.env` file contains your credentials - keep it safe!

---

## ❓ FAQ

**Q: Why doesn't it work with my Ubiquiti SSO account?**  
A: The API only works with **local admin accounts**. SSO/Cloud accounts and 2FA are not supported for API access. Create a dedicated local-only admin account (see Credential Requirements section).

**Q: How often does it check?**  
A: Every hour as a failsafe, plus at the exact LED_ON_TIME and LED_OFF_TIME you configure.

**Q: Can I change the schedule frequency?**  
A: The schedule is optimized to run at your configured times. The hourly check is a failsafe to ensure LEDs are in the correct state.

**Q: Will it turn off LEDs for offline APs?**  
A: Yes, the script sends the command regardless of AP status.

**Q: Can I control just one AP out of many?**  
A: Yes! Use `AP_MACS=xx:xx:xx:xx:xx:xx` in `.env` to specify which AP(s).

**Q: Does it work with UniFi Dream Machine?**  
A: Yes, as long as you can access the UniFi Controller API with a local admin account.

**Q: Can I run this on a Raspberry Pi?**  
A: Yes! Just make sure Docker is installed.

**Q: Is it safe to disable 2FA for the admin account?**  
A: Create a **separate** local-only admin account (no remote access) just for this script. Keep your main admin account with 2FA enabled. Use a strong password for the API account.

---

## 🎯 Full Workflow Example

```bash
# 1. Copy and configure .env
cp .env.example .env
nano .env  # Add your credentials

# 2. Use the included docker-compose.yml (no changes needed!)
# The file is already in the repo and ready to use

# 3. Start the container
docker compose up -d

# 4. See what APs you have
docker compose exec unifi-led-scheduler php /app/list_aps.php

# 5. (Optional) Configure specific APs
nano .env  # Add AP_MACS=...
docker compose restart

# 6. Test it works
docker compose exec unifi-led-scheduler php /app/toggle_ap_led.php

# 7. Monitor
docker compose logs -f

# Done! 🎉
```

---

## 🙏 Credits

This project is made possible by the **[Art-of-WiFi/UniFi-API-client](https://github.com/Art-of-WiFi/UniFi-API-client)** PHP library.

**Special thanks to the Art of WiFi team** and all contributors for their excellent work in creating and maintaining this comprehensive UniFi Controller API client. Without their efforts in reverse-engineering and documenting the API, this project wouldn't exist.

- **UniFi API Client**: https://github.com/Art-of-WiFi/UniFi-API-client
- **API Documentation**: https://github.com/Art-of-WiFi/UniFi-API-client/blob/master/API_REFERENCE.md

---

**Last Updated**: January 2026

