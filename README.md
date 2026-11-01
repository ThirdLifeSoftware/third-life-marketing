# Third Life Software marketing

Source for thirdlifesoftware.com and the company one-pager.

## site/

The public website, a single static page. Hosted on the Azure web app
`thirdlifesoftware-site` (resource group `rg-deltarecon`, plan `plan-deltarecon`).
Custom domains and managed certificates are bound for `thirdlifesoftware.com` and
`www.thirdlifesoftware.com`. DNS stays at Squarespace.

Deploy (PowerShell, from the repo root):

```powershell
cd site
python -c "import zipfile; z=zipfile.ZipFile('../site.zip','w',zipfile.ZIP_DEFLATED); [z.write(f,f) for f in ['index.html','favicon.svg','robots.txt','sitemap.xml']]; z.close()"
cd ..
az webapp deploy -g rg-deltarecon -n thirdlifesoftware-site --src-path site.zip --type zip --clean true
Remove-Item site.zip
```

Zip with Python, not Compress-Archive, so the archive uses forward slashes.

## one-pager/

The company one-pager at US Letter. `index.html` is the print-ready source;
open it in a browser and print to PDF, or regenerate the PDF with headless Edge:

```powershell
& "C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe" --headless=new --disable-gpu --user-data-dir="$env:TEMP\edge-pdf" --no-first-run --no-pdf-header-footer --virtual-time-budget=10000 --print-to-pdf="$PWD\one-pager\Third-Life-Software-one-pager.pdf" "file:///$($PWD -replace '\\','/')/one-pager/index.html"
Get-Item .\one-pager\Third-Life-Software-one-pager.pdf | Select-Object LastWriteTime
```

The separate `--user-data-dir` matters. Without it, an open Edge window takes the
request and nothing is written. Check the timestamp after running it.

Both pieces were designed on a Claude Design canvas; the HTML here is the hand-built
version and is the source of record.

## Copy rules

Sentence case. No em dashes. Plain sentences, no marketing filler. Facts a reader
can check: 20+ years, 50+ funds, 4x faster lifecycle, 400 hours for the Geneva
migration against 1,500 to 2,000 by traditional methods.
