# PicoCTF Heap-Dump Challenge Write-Up

## Challenge Overview
The "head-dump" challenge is a web exploitation task in PicoCTF, categorized under Web Exploitation, worth 100 points. It involves a vulnerable Node.js website where you can trigger a memory heap dump and extract a hidden flag from it. The challenge provides a live instance URL, and the goal is to find the flag in the format `picoCTF{...}` embedded in the heap snapshot.

## Prerequisites
- Basic knowledge of web requests (HTTP methods like HEAD, GET) and JSON parsing.
- Tools: curl (for web requests), grep or jq (for searching files), Python (for JSON handling), Chrome DevTools (optional for visual analysis).
- No advanced forensics tools needed; command-line utilities suffice.

## Step-by-Step Solution

### Step 1: Initial Exploration
Access the challenge instance website. The homepage may have minimal content, but explore links like "/api-docs" which loads Swagger UI for API documentation. Read through the docs carefully—look for sections on diagnostics or troubleshooting that mention heap dumps.

### Step 2: Discovering the Vulnerability/Endpoint
In the API docs, find the "/heapdump" endpoint (often under a "Diagnosing" or similar section). Note that the challenge name "head-dump" hints at using the HTTP HEAD method. Test the endpoint with curl using HEAD to get metadata, then switch to GET to download the full dump.

### Step 3: Obtaining the Heap Dump
Use curl to request the heap dump:
- First, probe with HEAD: `curl -I <instance-url>/heapdump` to confirm existence.
- Then download: `curl <instance-url>/heapdump --output heapdump.heapsnapshot`.
The file is a V8 heap snapshot in JSON format, potentially large and named something like "heapdump-<timestamp>.heapsnapshot".

### Step 4: Analyzing the Heap Dump
The snapshot contains sections like "nodes", "edges", and a "strings" array at the end where the flag is likely hidden as plain text.
- Use grep for quick search: `grep -i "picoCTF" heapdump.heapsnapshot`.
- If JSON parsing issues arise (e.g., large numbers), avoid full loading and stick to text search.
- For structured analysis, use Python to extract the "strings" array (handling potential truncation by redownloading if needed).

### Step 5: Extracting the Flag
Search the "strings" array for "picoCTF{"—the flag will appear as a full string like `picoCTF{heap_dumps_are_fun}`. Submit it on the PicoCTF platform.

## Tools and Commands Used
- curl for downloading: `curl <url>/heapdump --output heapdump.heapsnapshot`.
- grep for searching: `grep "picoCTF" heapdump.heapsnapshot`.
- jq (if no parse errors): `jq '.strings[]' heapdump.heapsnapshot | grep "picoCTF"`.

## Code Snippets
```bash
# Download the heap dump
curl http://example-instance.com/heapdump --output heapdump.heapsnapshot

# Search for the flag
grep "picoCTF" heapdump.heapsnapshot
```

```python
# Python script to search strings array
import json

filename = 'heapdump.heapsnapshot'
search_term = 'picoCTF'

with open(filename, 'r') as f:
    data = json.load(f)

strings_array = data.get('strings', [])
matches = [s for s in strings_array if search_term in s]

for match in matches:
    print(match)
```

## Lessons Learned
- Heap dumps can leak sensitive memory data like flags or secrets in misconfigured apps.
- Always check API docs for hidden debug endpoints.
- Text searching large JSON files is often sufficient for CTFs; handle parsing errors by redownloading or using regex.

## Flag
`picoCTF{example_flag_here}`
