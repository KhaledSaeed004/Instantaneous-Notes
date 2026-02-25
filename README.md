# Instantaneous Notes

A minimalist notepad that stores your data entirely in the URL. No database, no backend, no tracking.

## How it works
Your note is compressed and encoded into the URL hash on the fly. To "save" or "share" a note, you simply copy the URL. When someone opens the link, the browser decompresses the hash and restores the text instantly.

## Features
* **Zero Backend:** Host it anywhere (GitHub Pages, Vercel) or run it locally as a single `index.html` file.
* **Real-time Persistence:** Debounced updates ensure the URL reflects your current draft as you type.
* **File Integration:** Drag and drop `.txt` files directly into the editor or use the native **Open** button.
* **Native Saving:** Save notes directly to your device using the **modern File System Access API**.
* **Reactive UI:**
    * **Auto RTL:** Automatically detects Arabic/Hebrew script and flips text direction.
    * **Live Stats:** Real-time Line/Column tracking and character counts.

## Technical Implementation

The project utilizes modern browser APIs to handle data compression and file management without a server.

### 1. Data Compression Pipeline
To maximize the amount of text stored in a URL, the data follows this path:
* **JSON Stringify:** Wraps the title and body into a single object.
* **Gzip Compression:** Utilizes `CompressionStream("gzip")` to significantly reduce payload size.
* **URL-Safe Base64:** Converts binary data to Base64, replacing `+` and `/` with `-` and `_` to remain URL-compliant.

### 2. Encoding Logic
```javascript
const stream = new Blob([new TextEncoder().encode(text)])
  .stream()
  .pipeThrough(new CompressionStream("gzip"));

const buffer = await new Response(stream).arrayBuffer();
// Resulting buffer is converted to a URL-safe Base64 string
```

### 3. RTL Detection

The editor uses a regex check (/[\u0600-\u06FF]/) on the first character of the input to dynamically toggle between ltr and rtl directions, ensuring a pleasant experience for multi-lingual users.

### 4. File System Access

It prioritizes the showSaveFilePicker API, allowing users to "Save As" directly to their local file system, falling back to a standard Blob download for older browsers.

## Usage

*  **Clone/Website:** Just grab the index.html or Use the hosted github pages version.

*  **Write:** Start typing.

*  **Share:** Copy the URL and send it to anyone.
