---
description: ... or concrete use cases for secator.
---

# Examples

***

### **Find subdomains using `subfinder` and run HTTP probes using `httpx`**

{% tabs %}
{% tab title="CLI" %}
```bash
secator x subfinder -raw alibaba.com | secator x httpx -threads 30 -rl 10 
```
{% endtab %}

{% tab title="Python" %}
```python
from secator.tasks.recon import subfinder
from secator.tasks.http import httpx

host = 'alibaba.com'
subdomains = subfinder('alibaba.com', raw=True).run()
for probe in httpx(subdomains, threads=30, rate_limit=10):
    print('Found alive subdomain URL {url}[{status_code}]'.format(**probe))
```
{% endtab %}
{% endtabs %}

***

### **Find open ports and run `nmap`'s `vulscan` NSE script on results**

{% tabs %}
{% tab title="CLI" %}
```bash
secator w port_scan cnn.com
```
{% endtab %}

{% tab title="Python" %}
```python
from secator.runner import Workflow
from secator.template import TemplateLoader

host = 'cnn.com'
config = TemplateLoader(name='workflows/port_scan')
for result in Workflow(config):  # consume results live
    print(result)
```
{% endtab %}
{% endtabs %}

***

### **Fuzz URLs with multiple fuzzers and a custom wordlist**

{% tabs %}
{% tab title="CLI" %}
```bash
secator w url_fuzz example.com -mc 200,302 -rl 1 -w dicc.txt -o table -quiet 
```
{% endtab %}

{% tab title="Python" %}
```python
from secator.runner import Workflow
from secator.template import TemplateLoader
from secator.exporters import TableExporter

host = 'example.com'
opts = {
    'match_codes': '200, 302',
    'rate_limit': 1 # req/s
    'quiet': True,
    'ffuf.wordlist': 'dicc.txt' # ffuf wordlist
}

host = 'cnn.com'
config = TemplateLoader(name='workflows/url_fuzz')

# Print results live and a summary table at the end of the run
for result in Workflow(config, exporters=[TableExporter]):
    print(result)
```
{% endtab %}
{% endtabs %}

***
