# mpvscrobble - Scrobble from MPV to Last.fm 

## 🎯 Features

✅ Automatic ICY metadata parsing  
✅ Support for multiple title formats  
✅ Configurable scrobble thresholds  
✅ "Now Playing" updates  
✅ Local file support with full metadata  
✅ Error handling and recovery  
✅ Session caching  
✅ Smart duplicate prevention  

---

## 🚀 Installation 

### 1. Install the script
Copy `mpvscrobble.lua` to `~/.config/mpv/scripts/mpvscrobble.lua`

### 2. Create configuration file
Copy `./mpvscrobble.conf.example` to `~/.config/mpv/script-opts/mpvscrobble.conf`:

```ini
# Last.fm credentials (REQUIRED)
username=your_lastfm_username
password=your_lastfm_password

# Last.fm Apikey (REQUIRED)
apikey=your_apikey
secret=your_apisecret

# Scrobble threshold - song must play for 240 seconds OR 50% duration
scrobble_threshold=240
scrobble_percentage=0.5

# Enable/disable different source types
enable_radio=yes
enable_local=yes

# Try to parse "Artist - Title" patterns from ICY metadata
parse_patterns=yes
```

## 🎵 Usage

### Radio Streams
```bash
mpv https://radio.url/jazz --no-resume-playback
```

The script will:
- Detect the radio stream automatically
- Parse ICY metadata (e.g., "Where Do We Go from Here? by Ingrid Jensen")
- Extract artist and title
- Scrobble after the threshold is met

### Local Files
```bash
mpv ~/Music/song.mp3
```

The script will:
- Read metadata tags (artist, title, album)
- Scrobble after 50% playback or 4 minutes

## 🔧 Supported ICY Title Formats

The parser handles these common formats:

- ✅ `Title by Artist`
- ✅ `Artist - Title`
- ✅ `Title by Artist - Station Name` (station removed)
- ✅ `Artist - Title on Station` (station removed)

Examples:
- `Where Do We Go from Here? by Ingrid Jensen` → Artist: "Ingrid Jensen", Title: "Where Do We Go from Here?"
- `Ingrid Jensen - Where Do We Go from Here?` → Same result
- `Song Title by Artist Name - walmradio.com` → Station suffix removed

## 📊 Viewing Logs

To see what's happening:

```bash
# Run with verbose logging
mpv --msg-level=mpvscrobble=debug https://radio.url/jazz
```

Log messages you'll see:
- `New track detected: [title] by [artist]`
- `Now playing: "[title]" by [artist]`
- `Scrobbled "[title]" by [artist]`
- `Track eligible for scrobbling` (when threshold met)

## ⚙️ Configuration Options

| Option | Default | Description |
|--------|---------|-------------|
| `username` | (none) | Your Last.fm username |
| `password` | (none) | Your Last.fm password |
| `apikey`   | (none) | Your Last.fm api key  |
| `secret`   | (none) | Your Last.fm api secret |
| `scrobble_threshold` | 240 | Minimum seconds before scrobbling |
| `scrobble_percentage` | 0.5 | Minimum percentage (0.5 = 50%) |
| `enable_radio` | yes | Scrobble radio streams |
| `enable_local` | yes | Scrobble local files |
| `parse_patterns` | yes | Parse ICY title patterns |

## 🐛 Troubleshooting

### "Last.fm username not configured"
- Make sure `mpvscrobble.conf` exists in `~/.config/mpv/script-opts/`
- Check that username and password (plaintext) are set correctly

### No metadata detected from radio
- Some streams don't send ICY metadata
- Try: `mpv --http-header-fields="Icy-MetaData:1" <url>`
- Check with: `ffmpeg -i <url>` to see available metadata

### Songs not scrobbling
- Check logs with `--msg-level=mpvscrobble=debug`
- Ensure you're playing for at least 4 minutes or 50% of track
- Verify credentials are correct

## 📝 Notes

- The script uses Last.fm's mobile auth API (no need for API tokens)
- Only scrobbles tracks that meet the threshold (prevents accidental scrobbles)
- Works with playlists and radio streams
- Session is created automatically on first use
