# Google-Colab-Drive-Uploader

The script follows a simple decision tree:
1.  **Mounts Drive:** Connects to your personal Google Drive securely.
2.  **Input Handling:** Takes the `Target URL` and `Destination Folder Name`.
3.  **Protocol Selection:**
    * `Folder Link` → Trigger `gdown` (Recursive download)
    * `File Link` → Trigger `gdown` (Single file)
    * `Direct Link` → Trigger `wget` (Direct fetch)

## 🛠️ Usage
1.  Open a new Notebook in [Google Colab](https://colab.research.google.com/).
2.  Copy the code below into a cell.  OR You can download Drive_Uploader.ipynb from the Repository and save it in your drive and directly open it from here it will automaticly open google colab in chrome and then you can use it.
3.  Run the cell (`Shift + Enter`).
4.  Follow the prompts to authorize Drive and paste your link.

### The Script
```python
from google.colab import drive
import os

# 1. Connect Drive
drive.mount('/content/drive')

# 2. Get Inputs
link = input("\n🔗 Paste the Link: ").strip()
folder_name = input("📂 Enter Folder Name (in Drive): ").strip()

# 3. Prepare Path
save_path = f"/content/drive/My Drive/{folder_name}/"
if not os.path.exists(save_path):
    os.makedirs(save_path)

# 4. Install Tools
os.system("pip install -U --no-cache-dir gdown --pre")

# 5. Auto-Detect & Download
print("\n⏳ Processing started...")

if "drive.google.com" in link and "folders" in link:
    # Drive Folder Logic
    os.system(f'gdown --folder "{link}" -O "{save_path}"')

elif "drive.google.com" in link:
    # Drive File Logic
    os.system(f'gdown "{link}" -O "{save_path}"')

else:
    # Direct Link Logic (Non-Drive)
    os.system(f'wget -P "{save_path}" "{link}"')

print(f"\n✅ Done! Files saved in '{folder_name}' folder.")
