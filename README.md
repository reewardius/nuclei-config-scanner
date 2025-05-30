# nuclei-config-scanner
#### Reconnaissance and Detection of Active HTTP Services
```bash
subfinder -dL root.txt -all -silent -o subs.txt
naabu -l subs.txt -s s -tp 100 -ec -c 50 -o naabu.txt
httpx -l naabu.txt -rl 500 -t 200 -o alive_http_services.txt
```
1. **subfinder**: Enumerates subdomains for all domains listed in root.txt and outputs the results to `subs.txt`.
2. **naabu**: Performs a fast port scan (TCP SYN) on the resolved subdomains from subs.txt to identify hosts with open ports and saves the results to `naabu.txt`.
3. **httpx**: Probes the hosts from naabu.txt to detect live HTTP services and writes the output to `alive_http_services.txt`.

#### Filter Out Targets Behind WAF and Run Configuration Scanning
```bash
cdncheck -i subs.txt -fwaf -resp -o no_waf.txt
nuclei -l no_waf.txt -tags config,exposure -rl 500 -c 200 -o nuclei-config.txt
```
4. **cdncheck**: Identifies subdomains not protected by a WAF/CDN using response analysis and writes the filtered list to `no_waf.txt`.
5. **nuclei**: Executes configuration and exposure vulnerability scans on the filtered targets using specific tags, saving the findings to `nuclei-config.txt`.

