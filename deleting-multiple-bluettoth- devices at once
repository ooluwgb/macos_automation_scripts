Absolutely! Here's a detailed and user-friendly **Markdown explanation** for your repository that explains the process of listing and removing Bluetooth devices using a Bash automation script on macOS.

---

```markdown
# 🔗 macOS Bluetooth Manager (CLI Automation Script)

This guide provides a **command-line solution** to list and selectively **remove/unpair Bluetooth devices** on macOS using a Bash script and a lightweight tool called `blueutil`.

> ⚠️ This script is intended for macOS users who prefer terminal-based system control or want to automate Bluetooth device management.

---

## 📦 Prerequisites

1. **macOS** with Terminal access
2. [**Homebrew**](https://brew.sh) installed
3. Install `blueutil` via Homebrew:
   ```bash
   brew install blueutil
   ```

---

## 🔍 What This Script Does

- Lists all paired Bluetooth devices.
- Compares each device name against a **whitelist of devices to keep**.
- Unpairs all **non-whitelisted** devices.
- Logs actions to the terminal for easy debugging.

---

## ✍️ The Script

```bash
#!/bin/bash

# List of Bluetooth device names you want to keep
keep_list=("Bose S1 Pro" "MX Ergo" "Keyboard K380")

# Check if blueutil is installed
if ! command -v blueutil &> /dev/null; then
  echo "❌ Error: blueutil is not installed. Please run: brew install blueutil"
  exit 1
fi

# Get all paired device MAC addresses
paired_devices=$(blueutil --paired | awk -F'[:,]' '/address:/ {print $2}' | xargs)

# Loop through each paired device
for addr in $paired_devices; do
  # Fetch the device name
  name=$(blueutil --info "$addr" | grep 'name:' | cut -d '"' -f2)

  # Handle unnamed or offline devices
  if [[ -z "$name" ]]; then
    echo "⚠️  Skipping unnamed or offline device: $addr"
    continue
  fi

  # Check if the device should be kept
  keep_device=false
  for keep in "${keep_list[@]}"; do
    if [[ "$name" == "$keep" ]]; then
      keep_device=true
      break
    fi
  done

  # Unpair if not in keep list
  if [[ "$keep_device" == false ]]; then
    echo "🗑️  Unpairing device: $name ($addr)"
    blueutil --unpair "$addr"
    if [[ $? -ne 0 ]]; then
      echo "❌ Failed to unpair $name ($addr). Please check permissions or device state."
    else
      echo "✅ Successfully unpaired: $name"
    fi
  else
    echo "✅ Keeping device: $name ($addr)"
  fi
done
```

---

## 🛠️ Error Handling & Debugging Tips

| Problem | Possible Cause | Fix |
|--------|----------------|-----|
| `blueutil: command not found` | `blueutil` not installed | Run `brew install blueutil` |
| Script skips device with no name | Device is offline or name is hidden | Turn device on, or check in GUI first |
| `--unpair` fails | macOS permission issues or system policy | Ensure Terminal has Bluetooth permissions in System Settings > Privacy & Security |
| Keeps wrong devices | Typos in `keep_list` | Double-check exact names from `system_profiler SPBluetoothDataType` |

---

## 📘 Example Output

```
✅ Keeping device: MX Ergo (E6:21:66:FF:A4:89)
🗑️  Unpairing device: Bluetooth Mouse (F1:4C:FF:CA:93:7D)
✅ Successfully unpaired: Bluetooth Mouse
⚠️  Skipping unnamed or offline device: 90:9C:4A:CC:9E:7B
```

---

## 🧼 Optional: Dry Run Mode

To test what the script would do **without making changes**, you can add a `DRY_RUN=true` flag and modify the unpair section like this:

```bash
if [[ "$DRY_RUN" == true ]]; then
  echo "🧪 [Dry Run] Would unpair $name ($addr)"
else
  blueutil --unpair "$addr"
fi
```

---

## 🙌 Contributions Welcome

Feel free to fork, improve, and submit pull requests to help others manage Bluetooth on macOS better!

```

---

Let me know if you want this as a downloadable `.md` file or embedded in a full `README.md` template with license, badges, and contribution guide!
