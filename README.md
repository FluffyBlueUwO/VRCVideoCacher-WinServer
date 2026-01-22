# VRCVideoCacher

### What is VRCVideoCacher?

VRCVideoCacher is a tool used to cache VRChat videos to your local disk and/or fix YouTube videos from failing to load.

### How does it work?

It replaces VRChats yt-dlp.exe with our own stub yt-dlp, this gets replaced on application startup and is restored on exit.

Auto install missing codecs: [VP9](https://apps.microsoft.com/detail/9n4d0msmp0pt) | [AV1](https://apps.microsoft.com/detail/9mvzqvxjbq9v) | [AC-3](https://apps.microsoft.com/detail/9nvjqjbdkn97)

### Are there any risks involved?

From VRC or EAC? no.

From YouTube/Google? maybe, we strongly recommend you use an alternative Google account if possible.

### How to circumvent YouTube bot detection

In order to fix YouTube videos failing to load, you'll need to install our Chrome extension from [here](https://chromewebstore.google.com/detail/vrcvideocacher-cookies-ex/kfgelknbegappcajiflgfbjbdpbpokge) or Firefox from [here](https://addons.mozilla.org/en-US/firefox/addon/vrcvideocachercookiesexporter), more info [here](https://github.com/clienthax/VRCVideoCacherBrowserExtension). Visit [YouTube.com](https://www.youtube.com) while signed in, at least once while VRCVideoCacher is running, after VRCVideoCacher has obtained your cookies you can safely uninstall the extension, although be aware that if you visit YouTube again with the same browser while the account is still logged in, YouTube will refresh you cookies invalidating the cookies stored in VRCVideoCacher. To circumvent this I recommended deleting your YouTube cookies from your browser after VRCVideoCacher has obtained them, or if you're using your main YouTube account leave the extension installed, or maybe even use an entirely separate web browser from your main one to keep things simple.

### Fix YouTube videos sometimes failing to play

> Loading failed. File not found, codec not supported, video resolution too high or insufficient system resources.

Sync system time, Open Windows Settings -> Time & Language -> Date & Time, under "Additional settings" click "Sync now"

Edit `Config.json` and set `ytdlDelay` to something like `10` seconds.

### Fix cached videos failing to play in public instances

> Attempted to play an untrusted URL (Domain: localhost) that is not allowlisted for public instances.

Run notepad as Admin then browse to `C:\Windows\System32\drivers\etc\hosts` add this new line `127.0.0.1 localhost.youtube.com` to the bottom of the file, edit `Config.json` and set `ytdlWebServerURL` to `http://localhost.youtube.com:9696`

### Running on Windows Server 2022
1. Launch **Server Manager** from Start
2. Click **Manage** > **Add Roles and Features**
3. Click **Next** until you reach the **Features** section
4. Check **Media Foundation** and **.NET Framework 3.5 Features**
5. Click **Next** and then **Install**
6. Wait until it says **Installation succeeded**
7. Close **Server Manager**
8. Launch **Windows PowerShell** from Start and run the following command:

```
Invoke-WebRequest https://raw.githubusercontent.com/asheroto/winget-installer/master/winget-install.ps1 -UseBasicParsing | iex
```

*This will install **winget**, which is required to obtain and verify the required codec(s).*

9. Go to https://store.rg-adguard.net
10. Copy and paste the Microsoft Store link for VP9 found below:

VP9: https://apps.microsoft.com/detail/9n4d0msmp0pt  
AV1: https://apps.microsoft.com/detail/9mvzqvxjbq9v  
AC-3: https://apps.microsoft.com/detail/9nvjqjbdkn97  

11. Click the checkmark
12. Download the file that ends with **.AppxBundle**
13. **Repeat steps 10–12 for AV1 and AC-3**
14. Return to **Windows PowerShell** (or reopen it if you closed it)
15. Navigate to the folder where you downloaded the **.AppxBundle** files

*It is important that you have enabled **File name extensions** in Windows Explorer.*

16. Update the following lines to match the **.AppxBundle version** you downloaded, then run the command:

```
Add-AppxPackage -Path "%appdata%\..\..\Downloads\Microsoft.VP9VideoExtensions_1.2.12.0_neutral_~_8wekyb3d8bbwe.AppxBundle"
Add-AppxPackage -Path "%appdata%\..\..\Downloads\Microsoft.AV1VideoExtension_2.0.6.0_neutral_~_8wekyb3d8bbwe.AppxBundle"
Add-AppxPackage -Path "%appdata%\..\..\Downloads\DolbyLaboratories.DolbyDigitalPlusDecoderOEM_1.2.581.0_neutral_~_rz1tebttyb220.AppxBundle"
```

17. Everything is set! You can use the browser extension above to fix the bot problem.

### Running on Linux

- Install `dotnet-runtime-10.0`
- Run with `./VRCVideoCacher`
- By default VRCVideoCacher will try to download and run its own binaries, but if you'd like to use your system packages instead, set `ytdlPath` to `""` in `Config.json`, you'll need to install `deno`, `ffmpeg` and yt-dlp using `pip install "yt-dlp[default,curl-cffi]"` the pip version is required due to package manager versions not including browser impersonation, make sure to always keep yt-dlp updated otherwise you will run into issues.

### Uninstalling

- If you have VRCX, delete the startup shortcut "VRCVideoCacher" from `%AppData%\VRCX\startup`
- Delete "yt-dlp.exe" from `%AppData%\..\LocalLow\VRChat\VRChat\Tools` and restart VRChat or rejoin world.

### Config Options

| Option                    | Description                                                                                                                                                                                                                                                                                    |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ytdlWebServerURL          | Used to circumvent VRChats public world video player whitelist, see above for usage instructions.                                                                                                                                                                                              |
| ytdlPath                  | Path to the yt-dlp executable, default `Utils\\yt-dlp.exe`, when set to `""` it will use global PATH instead, as a side effect this will disable the yt-dlp, ffmpeg and deno auto updater.                                                                                                     |
| ytdlUseCookies            | Uses the [Chrome](https://github.com/clienthax/VRCVideoCacherBrowserExtension) or [Firefox](https://addons.mozilla.org/en-GB/android/addon/vrcvideocachercookiesexporter) extension for cookies, this is used to circumvent YouTubes bot detection.                                            |
| ytdlAutoUpdate            | Auto update yt-dlp, ffmpeg and deno.                                                                                                                                                                                                                                                           |
| ytdlAdditionalArgs        | Add your own [yt-dlp args](https://github.com/yt-dlp/yt-dlp?tab=readme-ov-file#usage-and-options) (only add these if you know what you're doing)                                                                                                                                               |
| ytdlDubLanguage           | Set preferred audio language for AVPro and cached videos, be warned you may end up with auto translated slop. e.g. `de` for German, check list of [supported lang codes](https://github.com/yt-dlp/yt-dlp/blob/c26f9b991a0681fd3ea548d535919cec1fbbd430/yt_dlp/extractor/youtube.py#L381-L390) |
| ytdlDelay                 | No delay (Default) `0`, YouTube videos can fail to load sometimes in-game without this delay. e.g. `8` for 8 seconds.                                                                                                                                                                          |
| CachedAssetPath           | Location to store downloaded videos, e.g. store videos on separate drive with `D:\\DownloadedVideos`                                                                                                                                                                                           |
| BlockedUrls               | List of URLs to never load in VRC, also works for blocking domains e.g. `[ "https://youtube.com", "https://youtu.be" ]` to block YouTube.                                                                                                                                                      |
| BlockRedirect             | Video to load in-place of Blocked URL.                                                                                                                                                                                                                                                         |
| CacheYouTube              | Download YouTube videos to `CachedAssets` to improve load times next time the video plays.                                                                                                                                                                                                     |
| CacheYouTubeMaxResolution | Maximum resolution to cache youtube videos in (Larger resolutions will take longer to cache), e.g. `2160` for 4K.                                                                                                                                                                              |
| CacheYouTubeMaxLength     | Maximum video duration in minutes, e.g. `60` for 1 hour.                                                                                                                                                                                                                                       |
| CacheMaxSizeInGb          | Maximum size of `CachedAssets` folder in GB, `0` for Unlimited.                                                                                                                                                                                                                                |
| CachePyPyDance            | Download videos that play while you're in [PyPyDance](https://vrchat.com/home/world/wrld_f20326da-f1ac-45fc-a062-609723b097b1)                                                                                                                                                                 |
| CacheVRDancing            | Download videos that play while you're in [VRDancing](https://vrchat.com/home/world/wrld_42377cf1-c54f-45ed-8996-5875b0573a83)                                                                                                                                                                 |
| PatchResonite             | Enable Resonite support.                                                                                                                                                                                                                                                                       |
| PatchVRC                  | Enable VRChat support.                                                                                                                                                                                                                                                                         |
| AutoUpdate                | When a update is available for VRCVideoCacher it will automatically be installed.                                                                                                                                                                                                              |
| PreCacheUrls              | Download all videos from a JSON list format e.g. `[{"fileName":"video.mp4","url":"https:\/\/example.com\/video.mp4","lastModified":1631653260,"size":124029113},...]` "lastModified" and "size" are optional fields used for file integrity.                                                   |

> Generate PoToken has unfortunately been [deprecated](https://github.com/iv-org/youtube-trusted-session-generator?tab=readme-ov-file#tool-is-deprecated)
