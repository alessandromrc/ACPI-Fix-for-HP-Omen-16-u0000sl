
> [!WARNING]
> BIOS version **F36** still uses the same ACPI tables as **F35**, so you **do not need to update** your overridden DSDT after upgrading.

# ACPI Fix for HP Omen 16-u0000sl

HP ships their laptops with broken ACPI for whatever reason.
Their BIOS probably doesn’t even know Linux exists and assumes everything is Windows… classic.

If you’re one of the people responsible for this decision at HP, **this video is for you:**
👉 [**Watch This.**](https://youtu.be/mOk3Ct4GD1M?t=22)

> [!TIP]  
> Need help or want to chat? Join our **official Discord server** for support, questions, or to show off your RGB setups:  
> **[➡️ Click here to join Discord](https://discord.gg/8UwyAJ7sBH)**  

---

## 🛠️ How to Recreate This Yourself

If you want to patch your own ACPI tables, follow the Arch Wiki guide:
🔗 [Arch Linux DSDT Guide](https://wiki.archlinux.org/title/DSDT)

To dump your ACPI tables, use **RwEverything** inside Windows to grab the currently working ones.

> [!WARNING]
> Since RwEverything seems to not be working on some systems you can use  [**SSDTTime**](https://github.com/corpnewt/SSDTTime)

1. Download **SSDTTime**
2. Select "P"
3. In the folder "Results" you should find "SSDT.aml", that will be you SSDT table that you need to use.

#### You can use **iasl** to disassemble / assemble the aml to dsl and modify it.
---

## 📥 Installation

### Get the Files

| Method       | Command |
|-------------|---------|
| **Clone Repo** | `git clone https://github.com/alessandromrc/OMEN-16-u0000sl-DSDT.git && cd OMEN-16-u0000sl-DSDT/F35` |
| **Download ZIP** | [Click here](https://github.com/alessandromrc/OMEN-16-u0000sl-DSDT/archive/main.zip), extract it, and `cd` into `F35` |

---

### 2 Copy the ACPI Patch

```bash
sudo mkdir -p /etc/initcpio/acpi_override
sudo cp DSDT.aml /etc/initcpio/acpi_override/
```

---

### 3 Edit mkinitcpio.conf

```bash
sudo nano /etc/mkinitcpio.conf
```

Find the `HOOKS` line and **add `acpi_override`**, so it looks something like this:

```bash
HOOKS=(base udev autodetect modconf block filesystems keyboard acpi_override fsck)
```

---

### 4 Rebuild Initramfs

```bash
sudo mkinitcpio -P
```

---

### 5 Known issues

- Make sure to keep Nvidia Optimus enabled on BIOS otherwise brightness control won't be available.

(also yes... touchpad and audio work... bluetooth also does... can you imagine!?)

## 🚀 Done!

Reboot your system, and you should be good to go.
