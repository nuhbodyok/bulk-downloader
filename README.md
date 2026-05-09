# Bulk URL Downloader

[![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)]()
[![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-F7DF1E?logo=javascript&logoColor=black)]()
[![Tailwind CSS](https://img.shields.io/badge/Tailwind%20CSS-06B6D4?logo=tailwindcss&logoColor=white)]()

A robust, client-side bulk file downloader that processes multiple URLs and downloads them with concurrency control, validation, and filtering — all in a single HTML file with zero build steps.

![Screenshot](https://via.placeholder.com/800x400/5D5CDE/FFFFFF?text=Bulk+URL+Downloader)

## Features

- **Multiple Input Methods**
  - Paste URLs directly (one per line)
  - Upload `.txt`, `.csv`, `.md`, or `.json` files
  - Combine a base URL with multiple paths
  - Auto-extract URLs from pasted text

- **Smart Validation**
  - HEAD request validation with timeout handling
  - CORS-aware fallback for cross-origin URLs
  - File type detection via extension and MIME type
  - Content-length reporting where available

- **Download Management**
  - Concurrent download limiting (1–10)
  - Configurable delay between downloads (100ms–5s)
  - Individual and batch download controls
  - Retry failed downloads

- **Filtering & Organization**
  - Filter by file type: Images, Videos, Audio, PDF, Documents, Data, Archives
  - Real-time URL count display
  - Clean filename sanitization (removes invalid characters)

- **Media Previews**
  - Hover-to-preview for images (with CORS handling)
  - File type badges and size indicators

- **Accessibility & UX**
  - Dark mode support (auto-detects system preference)
  - Keyboard shortcuts: `Ctrl+Enter` to process, `Esc` to clear
  - Toast notifications with queue management
  - Responsive design (mobile-friendly)

## Quick Start

### Option 1: Open Directly
Simply open `Bulk_File_Downloader.html` in any modern browser. No server required.

```bash
# Clone the repository
git clone https://github.com/yourusername/bulk-url-downloader.git

# Open in browser
cd bulk-url-downloader
open Bulk_File_Downloader.html        # macOS
xdg-open Bulk_File_Downloader.html    # Linux
start Bulk_File_Downloader.html       # Windows
```

### Option 2: Serve Locally (recommended for CORS-heavy usage)
```bash
# Python 3
python -m http.server 8000

# Node.js
npx serve .

# PHP
php -S localhost:8000
```

Then navigate to `http://localhost:8000/Bulk_File_Downloader.html`

## Usage Guide

### 1. Enter URLs
Paste your URLs into the text area, one per line:
```
https://example.com/document.pdf
https://example.com/photo.jpg
https://example.com/audio.mp3
```

### 2. Process
Click **Process URLs** or press `Ctrl+Enter`. The app will:
- Validate each URL format
- Send HEAD requests to check accessibility
- Detect file types
- Display results with status indicators

### 3. Filter (Optional)
Use the **File Type Filters** to show only specific types (e.g., only Images).

### 4. Download
- Click **Download** on individual items
- Or click **Download All** to batch download with configurable concurrency and delay

## Technical Details

### Architecture
- **Single-file application**: All HTML, CSS, and JavaScript in one `.html` file
- **Zero dependencies**: Only uses Tailwind CSS via CDN for styling
- **Client-side only**: No server, no API keys, no data collection

### Browser Compatibility
| Feature | Chrome | Firefox | Safari | Edge |
|---------|--------|---------|--------|------|
| Basic downloads | ✅ 90+ | ✅ 90+ | ✅ 14+ | ✅ 90+ |
| Fetch + Blob downloads | ✅ 90+ | ✅ 90+ | ✅ 14+ | ✅ 90+ |
| Clipboard API | ✅ 90+ | ✅ 90+ | ✅ 14+ | ✅ 90+ |
| AbortController | ✅ 90+ | ✅ 90+ | ✅ 14+ | ✅ 90+ |

### Security Considerations
- **XSS Protection**: All user input is sanitized before DOM insertion
- **CORS Handling**: Gracefully falls back when servers don't allow cross-origin requests
- **No External Data**: URLs are processed locally; nothing is sent to third-party servers
- **Filename Sanitization**: Removes characters that could be exploited (`<`, `>`, `:`, `"`, etc.)

### Limitations
- **Cross-origin downloads**: Browsers restrict forced downloads from other domains. The app attempts `fetch()` + `Blob` first, then falls back to the browser's default handling (which may open in a new tab)
- **Large files**: Files are streamed via browser download; no progress bar for individual files
- **Authentication**: URLs with embedded credentials (`https://user:pass@...`) will work but credentials are visible in the UI

## Configuration

The app includes sensible defaults that can be adjusted in the `CONFIG` object:

```javascript
const CONFIG = {
    MAX_URLS: 1000,                    // Maximum URLs per batch
    MAX_FILE_SIZE: 10 * 1024 * 1024, // Upload size limit (10MB)
    VALIDATION_TIMEOUT: 5000,        // HEAD request timeout (ms)
    MAX_CONCURRENT_VALIDATION: 5,    // Simultaneous validation requests
    MAX_CONCURRENT_DOWNLOADS: 10,    // Simultaneous downloads
    DEFAULT_DOWNLOAD_DELAY: 500,   // Delay between downloads (ms)
    TOAST_MAX_COUNT: 5,              // Maximum visible notifications
    TOAST_DURATION: 3000            // Notification display time (ms)
};
```

## File Structure

```
bulk-url-downloader/
├── Bulk_File_Downloader.html    # Main application (single file)
├── README.md                     # This file
└── LICENSE                       # MIT License
```

## Development

No build step required. To modify:

1. Open `Bulk_File_Downloader.html` in your editor
2. Edit the `<script>` section
3. Refresh in browser

### Code Organization
```
┌─ CONFIG & CONSTANTS       # Tunable parameters and mappings
├─ STATE MANAGEMENT        # Centralized state object
├─ DOM ELEMENT REFERENCES  # Cached query selectors
├─ UTILITY FUNCTIONS       # URL parsing, validation, sanitization
├─ TAB MANAGEMENT          # Input method switching
├─ FILE UPLOAD             # Drag & drop + file reading
├─ URL COMBINATION         # Base URL + path merging
├─ PASTE HANDLING          # Auto-extract from clipboard
├─ FILTER MANAGEMENT       # File type filtering
├─ STATS & UI UPDATES      # Progress tracking
├─ URL PROCESSING          # Validation and card generation
├─ DOWNLOAD MANAGEMENT     # Concurrent download queue
└─ EVENT HANDLERS          # User interactions
```

## Troubleshooting

### "Download failed" for valid URLs
- **Cause**: The target server doesn't allow cross-origin requests (CORS)
- **Solution**: The browser will open the file in a new tab instead. Save manually from there.
- **Alternative**: Use a browser extension or download manager for unrestricted cross-origin downloads.

### "Validation timed out"
- **Cause**: Server is slow or blocking HEAD requests
- **Solution**: Uncheck "Validate URL accessibility" to skip HEAD requests

### "No valid URLs found" from file upload
- **Cause**: File contains malformed URLs or unsupported format
- **Solution**: Ensure one URL per line, starting with `http://` or `https://`

### Dark mode not working
- The app follows your system preference. Toggle your OS dark mode setting.

## Contributing

Contributions welcome! Areas for improvement:

- [ ] Service Worker for offline support
- [ ] IndexedDB for URL list persistence
- [ ] ZIP packaging of downloaded files (using JSZip)
- [ ] Custom headers for authenticated downloads
- [ ] Export results as CSV/JSON

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

Distributed under the MIT License. See `LICENSE` for more information.

## Acknowledgments

- [Tailwind CSS](https://tailwindcss.com/) for utility-first styling
- [Heroicons](https://heroicons.com/) for the SVG icon set
- Browser vendors for the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) and [Download attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a#attr-download) specifications

---

**Disclaimer**: This tool is for downloading files you have permission to access. Respect robots.txt, terms of service, and copyright laws. The authors are not responsible for misuse.
