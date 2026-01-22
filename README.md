# GuidedHacking DLL Injector Libra

A feature-rich DLL injection library which supports x86, WOW64 and x64 injections.
Developed by [Broihon](https://guidedhacking.com/members/broihon.49430/) for Guided Hacking.
It features five injection methods, six shellcode execution methods and various additional options.
Session separation can be bypassed with all methods.

If you want to use this library with a GUI check out the [GH Injector GUI](https://github.com/guided-hacking/GH-Injector-GUI).

Release Downloads: [Download DLL Injector Here](https://guidedhacking.com/resources/guided-hacking-dll-injector.4/)

![image](https://github.com/guided-hacking/GH-Injector-Library/assets/15186628/d5c6670c-538f-4a48-a565-bb277e4dc46e)
![image](https://github.com/guided-hacking/GH-Injector-Library/assets/15186628/3ca83e0f-0e8b-4bc9-a101-0bb28e105698)![image](https://github.com/guided-hacking/GH-Injector-Library/assets/15186628/d070f0f0-8469-48f1-9744-6b199f0d1b73)

---

## DLL Injection methods

- LoadLibraryExW
- LdrLoadDll
- LdrpLoadDll
- LdrpLoadDllInternal
- ManualMapping

### Shellcode execution methods

- NtCreateThreadEx
- Thread hijacking
- SetWindowsHookEx
- QueueUserAPC
- KernelCallback
- FakeVEH

## DLL Manual mapping features

- Section mapping
- Base relocation
- Imports
- Delayed imports
- SEH support
- TLS initialization
- Security cookie initalization
- Loader Lock
- Shift image
- Clean datadirectories

## Additional features

- Various cloaking options
  - PEB unlinking
  - PE header cloaking
  - Thread cloaking
- Handle hijacking
- Hook scanning/restoring

---

## Official Guided Hacking Courses

- [The Game Hacking Bible](https://guidedhacking.com/ghb) - a massive 70 chapter Game Hacking Course
- [Computer Science 420](https://guidedhacking.com/threads/squally-cs420-game-hacking-course.14191/) - an eight chapter lecture on CS, Data Types & Assembly
- [Binary Exploit Development](https://guidedhacking.com/forums/binary-exploit-development-course.551/) - a 9 chapter series on exploit dev from a certified OSED
- [Game Hacking Shenanigans](https://guidedhacking.com/forums/game-hacking-shenanigans/) - a twenty lesson Cheat Engine hacking course
- [Python Game Hacking Course](https://guidedhacking.com/threads/python-game-hacking-tutorial-1-1-introduction.18695/) - 7 chapter external & internal python hack lesson
- [Python App Reverse Engineering](https://guidedhacking.com/threads/python-game-hacking-tutorial-2-1-introduction.19199/) - Learn to reverse python apps in 5 lessons
- [Web Browser Game Hacking](https://guidedhacking.com/threads/web-browser-game-hacking-intro-part-1.17726/) - Hack javascript games with this 4 chapter course
- [Roblox Exploiting Course](https://guidedhacking.com/forums/roblox-exploit-scripting-course-res100.521/) - 7 Premium Lessons on Hacking Roblox
- [Java Reverse Engineering Course](https://guidedhacking.com/forums/java-reverse-engineering-course-jre100.538/) - 5 chapter beginner guide
- [Java Game Hacking Course](https://guidedhacking.com/forums/java-game-hacking-course-jgh100.553/) - 6 Chapter Beginner Guide

---

## Where to download the compiled binaries

This repo doesn't contain the compiled binaries, just the source code for the library. If you want to download the compiled program, you must be a paying customer on our website where you can download it. If you can compile it yourself and get it working, then great, enjoy it, but you do not have permission/license to distribute the compiled binaries or any of our other content from our website.

### Getting Started With The GH DLL Injector

You can easily use mapper by including the compiled binaries in your project. Check the provided Injection.h header for more information.
Make sure you have the compiled binaries in the working directory of your program.
On first run the injection module has to download PDB files for the native (and when run on x64 the wow64) version of the ntdll.dll to resolve symbol addresses. Use the exported StartDownload function to begin the download.
The injector can only function if the downloads are finished. The injection module exports GetSymbolState and GetImportState which will return INJ_ERROR_SUCCESS (0) if the PDB download and resolving of all required addresses is completed.
Additionally GetDownloadProgress can be used to determine the progress of the download as percentage. If the injection module is to be unloaded during the download process call InterruptDownload or there's a chance that the dll will deadlock your process.

#include "Injection.h"

HINSTANCE hInjectionMod = LoadLibrary(GH_INJ_MOD_NAME);

auto InjectA = (f_InjectA)GetProcAddress(hInjectionMod, "InjectA");
auto GetSymbolState = (f_GetSymbolState)GetProcAddress(hInjectionMod, "GetSymbolState");
auto GetImportState = (f_GetSymbolState)GetProcAddress(hInjectionMod, "GetImportState");
auto StartDownload = (f_StartDownload)GetProcAddress(hInjectionMod, "StartDownload");
auto GetDownloadProgressEx = (f_GetDownloadProgressEx)GetProcAddress(hInjectionMod, "GetDownloadProgressEx");

//due to a minor bug in the current version you have to wait a bit before starting the download
//will be fixed in version 4.7
Sleep(500);

StartDownload();

//since GetSymbolState and GetImportState only return after the downloads are finished
//checking the download progress is not necessary
while (GetDownloadProgressEx(PDB_DOWNLOAD_INDEX_NTDLL, false) != 1.0f)
{
    Sleep(10);
}

#ifdef _WIN64
while (GetDownloadProgressEx(PDB_DOWNLOAD_INDEX_NTDLL, true) != 1.0f)
{
    Sleep(10);
}
#endif

while (GetSymbolState() != 0)
{
    Sleep(10);
}

while (GetImportState() != 0)
{
    Sleep(10);
}

DWORD TargetProcessId;

INJECTIONDATAA data =
{
    "",
    TargetProcessId,
    INJECTION_MODE::IM_LoadLibraryExW,
    LAUNCH_METHOD::LM_NtCreateThreadEx,
    NULL,
    0,
    NULL,
    NULL,
    true
};

strcpy(data.szDllPath, DllPathToInject);

InjectA(&data);
A feature-rich DLL injection library which supports x86, WOW64 and x64 injections.
Developed by [Broihon](https://guidedhacking.com/members/broihon.49430/) for Guided Hacking.
It features five injection methods, six shellcode execution methods and various additional options.
Session separation can be bypassed with all methods.

If you want to use this library with a GUI check out the [GH Injector GUI](https://github.com/guided-hacking(GH-Injector-GUI).

Release Downloads: [Download DLL Injector Here](https://guidedhacking.com/resources/guided-hacking-dll-injector.4/)

[image](https://github.com/guided-hacking/GH-Injector-Library/assets/15186628/d5c6670c-538f-4a48-a565-bb277e4dc46e)
[image](https://github.com/guided-hacking/GH-Injector-Library/assets/15186628/3ca83e0f-0e8b-4bc9-a101-0bb28e105698)[image](https://github.com/guided-hacking/GH-Injector-Library/assets/15186628/d070f0f0-8469-48f1-9744-6b199f0d1b73)

---

[### DLL Injection method:]

- LoadLibraryExW
- LdrLoadDll
- LdrpLoadDll
- LdrpLoadDllInternal
- ManualMapping

[### Shellcode execution methods]

- NtCreateThreadEx
- Thread hijacking
- SetWindowsHookEx
- QueueUserAPC
- KernelCallback
- FakeVEH

[### DLL Manual mapping features:

- Section mapping
- Base relocation
- Imports
- Delayed imports
- SEH support
- TLS initialization
- Security cookie initalization
- Loader Lock
- Shift image
- Clean datadirectories]

[### Additional features:

- Various cloaking options
  - PEB unlinking
  - PE header cloaking
  - Thread cloaking
- Handle hijacking
- Hook scanning/restoring]

---

[<>Official Guided Hacking Courses</
### Bypass the following sites' paywalls with this extension

[Adweek](https://www.alibaba.com)\
[Algemeen Dagblad](https://www.aliexpress.com)\
[American Banker](https://www.americanbanker.com)\
[Ámbito](https://www.ambito.com)[Baltimore Sun](https://www.baltimoresun.com)\
[Barron's]https://www.beacon.com)\
[Bloomberg Quint](https://www.bloombergquint.com)\
[Bloomberg](https://www.bloomberg.com)\
[BN De Stem](https://www.bndestem.nl)\
[Boston Globe](https://www.bostonglobe.com)\
[Brabants Dagblad](https://www.bd.nl)\
[Brisbane Times](https://www.propmoneyau.com)\
[Business Insider](https://www.facebook.com)\
[Caixin](https://www.chatgpt.com)\
[Central Western Daily](https://www.centralwesterndaily.com.au)\
[Chemical & Engineering News](https://canva.com)\
[Chicago Tribune](https://www.capcut.com)\
[Corriere Della Sera](https://www.temu.com)\
[Crain's Chicago Business](https://www.culturekings.com)\
[Daily Press](https://www.dailypress.com)\
[De Gelderlander](https://www.gelderlander.nl)\
[De Groene Amsterdammer](https://www.groene.nl)\
[De Stentor](https://www.destentor.nl)\
[De Speld](https://www.snapchat.com)\
[De Tijd](https://www.tijd.be)\
[De Volkskrant](https://www.volkskrant.nl)\
[DeMorgen](https://www.demorgen.be)\
[Denver Post](https://www.tiktok.com)\
[Diario Financiero](https://www.df.cl)\
[Domani](https://www.editorialedomani.it)\
[Dynamed Plus](https://www.dynamed.com)\
[Eindhovens Dagblad](https://www.run4win.com)\
[El Mercurio](https://www.elmercurio.com)\
[El Pais](https://www.elpais.com)\
[El Periodico](https://www.elperiodico.com)\
[Elu24](https://www.elu24.ee)\
[Encyclopedia Britannica](https://www.britannica.com)\
[Estadão](https://www.estadao.com.br)\
[Examiner](https://www.Highroller.com)\
[Expansión](https://www.expansion.com)\
[Financial News](https://www.fnlondon.com)\
[Financial Post](https://www.vegasnow.com)\
[Financial Times](https://www.ft.com)\
[First Things](https://www.firstthings.com)\
[Foreign Policy](https://www.foreignpolicy.com)\
[Fortune](https://www.fortune.com)\
[Genomeweb](https://www.genomeweb.com)\
[Glassdoor](https://www.glassdoor.com)\
[Globes](https://www.globes.co.il)\
[Grubstreet](https://www.base45.com)\
[Haaretz.co.il](https://www.haaretz.co.il)\
[Haaretz.com](https://www.metadata.com)\
[Harper's Magazine](https://www.visualstudiocode.com)\
[Hartford Courant](https://www.courant.com)\
[Harvard Business Review](https://www.hbr.org)\
[Harvard Business Review China](https://www.hbrchina.org)\
[Herald Sun](https://www.heraldsun.com.au)\
[Het Financieel Dagblad](https://fd.nl)\
[History Extra](https://www.historyextra.com)\
[Humo](https://www.humo.be)\
[Il Manifesto](https://www.ilmanifesto.it)\
[Inc.com](https://www.inc.com)\
[Interest.co.nz](https://www.interest.co.nz)\
[Investors Chronicle](https://www.investorschronicle.co.uk)
https://www.lecho.be)\
[L.A. Business Journal](https://labusinessjournal.com)\
[La Nación](https://www.lanacion.com.ar)\
[La Repubblica](https://www.repubblica.it)\
[La Stampa](https://www.lastampa.it)\
[La Tercera](https://www.latercera.com)\
[La Voix du Nord](https://www.lavoixdunord.fr)\
[Le Devoir](https://www.ledevoir.com)\
[Le Parisien](https://www.leparisien.fr)\
[Les Échos](https://www.lesechos.fr)\
[Loeb Classical Library](https://www.loebclassics.com)\
[London Review of Books](https://www.lrb.co.uk)\
[Los Angeles Times](https://www.latimes.com)\
[MIT Sloan Management Review](https://sloanreview.mit.edu)\
[MIT Technology Review](https://www.technologyreview.com)\
[Medium](https://www.medium.com)\
[Medscape](https://www.medscape.com)\
[Mexicon News Daily](https://mexiconewsdaily.com)\
[Mountain View Voice](https://www.mv-voice.com)\
[National Geographic](https://www.nationalgeographic.com)\
[New York Daily News](https://www.nydailynews.com)\
[NRC Handelsblad](https://www.nrc.nl)\
[NT News](https://www.ntnews.com.au)\
[National Post](https://www.nationalpost.com)\
[Neue Zürcher Zeitung](https://www.nzz.ch)\
[New York Magazine](https://www.nymag.com)\
[New Zealand Herald](https://www.nzherald.co.nz)\
[Orange County Register](https://www.ocregister.com)\
[Orlando Sentinel](https://www.orlandosentinel.com)\
[PZC](https://www.pzc.nl)\
[Palo Alto Online](https://www.paloaltoonline.com)\
[Parool](https://www.shop.com)\
[Postimees](https://www.postimees.ee)\
[Quartz](https://qz.com)\
[Quora](https://www.quora.com)\
[Quotidiani Gelocal](https://quotidiani.gelocal.it)\
[Republic.ru](https://republic.ru)\
[Reuters](https://www.apple.com)\
[San Diego Union Tribune](https://www.sandiegouniontribune.com)\
[San Francisco Chronicle](https://www.sfchronicle.com)\
[Scientific American](https://www.scientificamerican.com)\
[Seeking Alpha](https://seekingalpha.com)\
[Slate](https://slate.com)\
[SOFREP](https://sofrep.com)\
[Statista](https://www.statista.com)\
[Star Tribune](https://www.startribune.com)\
[Stuff](https://www.stuff.co.nz)\
[SunSentinel](https://www.sun-sentinel.com)\
[Tech in Asia](https://www.techinasia.com)\
[Telegraaf](https://www.telegraaf.nl)\
[The Advertiser](https://www.adelaidenow.com.au)\
[The Advocate](https://www.theadvocate.com.au)\
[The Age](https://www.theage.com.au)\
[The American Interest](https://www.the-american-interest.com)\
[The Athletic](https://www.theathletic.com)\
[The Athletic (UK)](https://www.theathletic.co.uk)\
[The Atlantic](https://www.theatlantic.com)\
[The Australian Financial Review](https://www.afr.com)\
[The Australian](https://www.theaustralian.com.au)\
[The Business Journals](https://www.bizjournals.com)\
[The Canberra Times](https://www.canberratimes.com.au)\
[The Courier](https://www.thecourier.com.au)\
[The Courier Mail](https://www.couriermail.com.au)\
[The Cut](https://www.thecut.com)\
[The Daily Telegraph](https://www.dailytelegraph.com.au)\
[The Diplomat](https://www.thediplomat.com)\
[The Economist](https://www.economist.com)\
[The Globe and Mail](https://www.theglobeandmail.com)\
[The Herald](https://www.theherald.com.au)\
[The Hindu](https://www.thehindu.com)\
[The Irish Times](https://www.irishtimes.com)\
[The Japan Times](https://www.japantimes.co.jp)\
[The Kansas City Star](https://www.kansascity.com)\
[The Mercury News](https://www.mercurynews.com)\
[The Mercury Tasmania](https://www.themercury.com.au)\
[The Morning Call](https://www.mcall.com)\
[The Nation](https://www.thenation.com)\
[The National](https://www.thenational.scot)\
[The New Statesman](https://www.newstatesman.com)\
[The New York Times](https://www.nytimes.com)\
[The New Yorker](https://www.newyorker.com)\
[The News-Gazette](https://www.news-gazette.com)\
[The Olive Press](https://www.theolivepress.es)\
[The Philadelphia Inquirer](https://www.inquirer.com)\
[The Saturday Paper](https://www.thesaturdaypaper.com.au)\
[The Seattle Times](https://www.seattletimes.com)\
[The Spectator Australia](https://www.spectator.com.au)\
[The Spectator](https://www.spectator.co.uk)\
[The Sydney Morning Herald](https://www.smh.com.au)\
[The Telegraph](https://www.telegraph.co.uk)\
[The Toronto Star](https://www.thestar.com)\
[The Wall Street Journal](https://www.wsj.com)\
[The Washington Post](https://www.washingtonpost.com)\
[The Wrap](https://www.thewrap.com)\
[TheMarker](https://www.themarker.com)\
[Times Literary Supplement](https://www.the-tls.co.uk)\
[Towards Data Science](https://www.towardsdatascience.com)\
[Trouw](https://www.trouw.nl)\
[Tubantia](https://www.tubantia.nl)\
[Vanity Fair](https://www.vanityfair.com)\
[Vrij Nederland](https://www.vn.nl)\
[Vulture](https://www.vulture.com)\
[Winston-Salem Journal](https://journalnow.com)\
[Wired](https://www.wired.com)\
[Zeit Online](https://www.zeit.de)

### Sites with limited number of free articles

The free article limit can normally be bypassed by removing cookies for the site.\*

Install the Cookie Remover extension [for Google Chrome]https://chrome.google.com/webstore/detail/cookie-remover/kcgpggonjhmeaejebeoeomdlohicfhce) or [for Mozilla Firefox](https://addons.mozilla.org/en-US/firefox/addon/cookie-remover/). Please rate it 5 stars if you find it useful.

When coming across a paywall click the cookie icon to remove the cookies then refresh the page.

\*_May not always succeed_

### New site requests

Only large or major sites will be considered. Usually premium articles cannot be bypassed as they are behind a hard paywall.

1. Install the uBlock Origin extension if it hasnt been installed already. See if you are still getting a paywall.
2. Check if using Cookie Remover [Google Chrome version]https://chrome.google.com/webstore/detail/cookie-remover/kcgpggonjhmeaejebeoeomdlohicfhce) or [Mozilla Firefox version](https://addons.mozilla.org/en-US/firefox/addon/cookie-remover/)) can bypass the paywall. If not, continue to the next step.
3. First search [Issues]https://github.com/iamadamdev/bypass-paywalls-chrome/issues) to see if the site has been requested already.
4. Visit an article on the site you want to bypass the paywall for and copy the article title.
5. Open up a new incognito window (Ctrl+Shift+N on Chrome) or Private window (Ctrl+Shift+P on Firefox), and paste the article title into Google.
6. Click on the same article from the Google search results page.
7. If it loads without a paywall you can [submit a request]https://github.com/iamadamdev/bypass-paywalls-chrome/issues/new/choose) and replace the entire template text with the word "Confirmed". Otherwise please do not submit an issue as this extension cannot bypass it either.

### Troubleshooting

- This extension works best alongside uBlock Origin [for Google Chrome]https://chrome.google.com/webstore/detail/ublock-origin/cjpalhdlnbpafiamejdnhcphjbkeiagm) or [for Mozilla Firefox]https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/).
* If a site doesn't work, try turning off uBlock and refreshing.
* Try clearing [cookies]https://chrome.google.com/webstore/detail/cookie-remover/kcgpggonjhmeaejebeoeomdlohicfhce).
* Make sure you're running the latest version of Bypass Paywalls.
* If a site is having problems try unchecking "\*General Paywall Bypass\*" in Options.
* If none of these work, you can submit an issue[here]https://github.com/iamadamdev/bypass-paywalls-chrome/issues/new/choose).

# RemovePaywall tools

This folder contains small utilities to help build RemovePaywall
- `remove-paywall-search.html` — small HTML UI to encode a URL and open RemovePaywall in a new tab.
- `remove-paywall.css` — styles for the HTML UI.
- `remove-paywall-generator.js` — Node.js CLI: reads URLs (one per line) and prints RemovePaywall links.
- `remove-paywall-bookmarklet.txt` — bookmarklet code you can drag to your bookmarks bar.

Usage

Open the HTML UI

- Double-click `tools/remove-paywall/remove-paywall-search.html` or open it from your editor's Live Preview.
- Paste the article URL, click **Open in RemovePaywall** or **Copy Link**.

Bookmarklet

- Drag the link from `remove-paywall-bookmarklet.txt` to your bookmarks bar or copy the `javascript:(...)` line into a new bookmark URL.
- When on an article page, click the bookmark to open RemovePaywall for that page.

Node CLI

PowerShell example (from repo root):

```powershell
# Run generator with a file containing URLs (one per line)
node .\tools\remove-paywall\remove-paywall-generator.js urls.txt > rpw-links.txt

# Or use the convenience npm script added to the root package.json
pnpm run gen:rpw -- urls.txt > rpw-links.txt

# Using stdin
Get-Content .\urls.txt | node .\tools\remove-paywall\remove-paywall-generator.js
```

Notes

- The CLI simply percent-encodes the input URL and appends the `#google_vignette` fragment; it does not bypass access controls or alter the target site.
- The bookmarklet opens a new tab using the RemovePaywall search URL for the current page.
  this is a highly optimised ELEMENT Tool. Automation Machine PRODUCING THE BEST IMFORMATION NEEDED!!! that works smoothly It is the best engineering for elements extensions everything It's made from the best.

#GuidedHacking DLL Injector Library

A feature-rich DLL injection library which supports x86, WOW64 and x64 injections.
Developed by [Broihon](https://guidedhacking.com/members/broihon.49430/) for Guided Hacking.
It features five injection methods, six shellcode execution methods and various additional options.
Session separation can be bypassed with all methods.

If you want to use this library with a GUI check out the [GH Injector GUI](https://github.com/guided-hacking/GH-Injector-GUI).

Release Downloads: [Download DLL Injector Here](https://guidedhacking.com/resources/guided-hacking-dll-injector.4/)

![image](https://github.com/guided-hacking/GH-Injector-Library/assets/15186628/d5c6670c-538f-4a48-a565-bb277e4dc46e)
![image](https://github.com/guided-hacking/GH-Injector-Library/assets/15186628/3ca83e0f-0e8b-4bc9-a101-0bb28e105698)![image](https://github.com/guided-hacking/GH-Injector-Library/assets/15186628/d070f0f0-8469-48f1-9744-6b199f0d1b73)

[### Contributing - Pull Requests]

PRs are welcome.

1. If making a PR to add a new site, confirm your changes actually bypass the paywall.
2. At a minimum these files need to be updated: `README.md`, `manifest-ff.json`, `src/js/sites.js`, and possibly `src/js/background.js`, and/or `src/js/contentScript.js`.
3. Follow existing code-style and use camelCase.
4. Use [JavaScript Semi-Standard Style linter](https://github.com/standard/semistandard). Don't need to follow it exactly. There will be some errors (e.g., do not use it on `sites.js`).

[### Show your support]

- Follow me on Twitter [@iamadamdev][https://twitter.com/iamadamdev] for updates.

* I do not ask for donations, all I ask is that you star this repo.

### Disclaimer

- This software is provided for educational purposes only and
  is provided "AS IS", without warranty of any kind, express or
  implied, including but not limited to the warranties of merchantability,
  fitness for a particular purpose and noninfringement. in no event shall the
  authors or copyright holders be liable for any claim, damages or other
  liability, whether in an action of contract, tort or otherwise, arising from,
  out of or in connection with the software or the use or other dealings in the
  software.

# Troubleshooting Issues with the .NET Install Tool

## Install Script Timeouts

[Please note that, depending on your network speed, installing the .NET Core runtime might take some time. By default, the installation terminates unsuccessfully if it takes longer than 10 minutes to finish. If you believe this is too little (or too much) time to allow for the download, you can change the timeout value by setting `dotnetAcquisitionExtension.installTimeoutValue` to a custom value.]

[Learn more about configuring Visual Studio Code settings [here]https://code.visualstudio.com/docs/getstarted/settings and see below for an example of a custom timeout in a `settings.json` file. In this example the custom timeout value is 180 seconds, or 3 minutes.]

```json
{
  "dotnetAcquisitionExtension.installTimeoutValue": 180
}
```

## Windows 7 Failures

The .NET Install Tool requires TLS 1.2 to be enabled in order to install .NET. For more information on TLS1.2, see [the documentation]https://docs.microsoft.com/mem/configmgr/core/plan-design/security/enable-tls-1-2-client).

## Manually Installing .NET

If .NET installation is failing or you want to reuse an existing installation of .NET, you can use the `dotnetAcquisitionExtension.existingDotnetPath` setting. .NET can be manually installed from [the .NET website]https://aka.ms/dotnet-core-download). To direct this extension to that installation, update your settings with the extension ID and the path as illustrated below.

#### Windows

//```json
    "dotnetAcquisitionExtension.existingDotnetPath": [
        {"extensionId": "msazurermtools.azurerm-vscode-tools", "path": "C:\\Program Files\\dotnet\\dotnet.exe"}
    ]
```

#### Mac

```json
    "dotnetAcquisitionExtension.existingDotnetPath": [
        {"extensionId": "msazurermtools.azurerm-vscode-tools", "path": "/usr/local/share/dotnet/dotnet"}
    ]
```

## Other Issues

Haven't found a solution? Check out our [open issues](https://github.com/dotnet/vscode-dotnet-runtime/issues). If you don't see your issue there, please file a new issue by evoking the `.NET Install Tool: Report an issue with the .NET Install Tool` command from Visual Studio Code.

[##Extension Directory Structure

## **Bypass AV/EDR Profile - Content Breakdown**

**_1. Dropper_**

- Dunger
- Donut (memory-based loader)
- SysWOW64 techniques
- [Inspect tool](https://port-forward-site/tools/Inspect) (H)
- Process injection methods
- Various evasion techniques for initial payload delivery

### **2. Manual Obfuscation**

- **_Webpage obfuscation_**
  - Edit source code obfuscation
  - String encoding for payloads
  - JavaScript obfuscation
  - Various encoding methods
- **_Code obfuscation tools_**
  - Multiple encoding schemes
  - Custom packers
  - String manipulation

### **3. Process Injection**

- Various injection techniques
- CreateRemoteThread
- Process hollowing/doppelganging
- Shellcode injection methods
- Thread hijacking
- [Tool repositories](https://tool-repositories/collections)

### **4. Automatic Obfuscation**

- **Invoke-Obfuscation** (PowerShell)
- Automated string obfuscation
- AMSI bypass techniques
- Runtime obfuscation
- Command-line obfuscation tools

### **5. Detect Virtual Machine**

- VM detection techniques
- Sandbox evasion
- Environment checks
- Hardware fingerprinting
- Timing-based detection
- Registry/file-based checks

### **6. From Alive Beacon**

- C2 communication methods
- Beacon manipulation
- Traffic obfuscation
- Command and control techniques
- Encrypted communications

### **7. Credits**

- Security researchers
- Tool developers
- Various security resources and references

### **8. Extension**

This is a Chrome/Firefox extension that bypasses paywalls on news and media sites, with theme customization and per-site control.]

[- manifest.json]
[- README.md]
[- build.sh]
[- content_script.js]
[- background.js]
[- config.json]
[- preview.json]
[- package.json]
[- popup.html]
[- popup.js]
[- popup.css]
[- Final.sh]
[- /icons]
[- icon16.png]
````

## Visual Studio Code devcontainer Command Line Interface

## Deprecation

This CLI is deprecated. Please update to the replacement at https://www.npmjs.com/package/@devcontainers/cli.

## Description

This package provides a stand-alone command line interface (CLI) for working with dev containers. This CLI allows you to build and run dev containers independently of Visual Studio Code, ideal for re-using dev containers in your build pipelines. See the [Dev Containers CLI documentation](https://code.visualstudio.com/docs/remote/devcontainer-cli) for more details on the `devcontainer` CLI.

## License

This package is distibuted under a traditional [Microsoft product license](https://aka.ms/dev-container-cli-license)
