# VirusTotalInfo

# Domain URL Checker Script

This script allows you to fetch and display undetected URLs for one or more domains using the VirusTotal API. It includes features such as API key rotation, error handling, and a countdown to comply with API rate limits.

---

## Features

1. **API Key Rotation**:
   - Automatically rotates among multiple API keys after a configurable number of requests.

2. **Input Options**:
   - Process a single domain or multiple domains from a file.

3. **Rate Limiting**:
   - Includes a countdown timer to comply with API rate limits.

4. **Error Handling**:
   - Handles network and API errors gracefully.
   - Provides detailed feedback if something goes wrong.

5. **Color-Coded Output**:
   - Clear and visually appealing output for different statuses:
     - **Blue**: Fetching data.
     - **Green**: Successfully fetched undetected URLs.
     - **Yellow**: No URLs found.
     - **Red**: Errors.

---

## Requirements

- **Bash** (Unix-based systems)
- **jq** (For JSON parsing)
- **curl** (For HTTP requests)

To install `jq` (if not already installed), run:
```bash
sudo apt-get install jq    # Debian/Ubuntu
sudo yum install jq        # CentOS/Red Hat
brew install jq            # macOS
```
# Usage
**Single Domain**
Fetch undetected URLs for a single domain:
```bash
./domain_checker.sh example.com
```
**Multiple Domains**
Fetch undetected URLs for a list of domains from a file
```bash
./domain_checker.sh -f domains.txt
```
The domains.txt file should contain one domain per line:
```
example.com
test.com
anotherdomain.com
```
**Help**
To display usage instructions:
```
./domain_checker.sh -h
```

## Configuration
You can customize the script by modifying the following variables located at the top of the domain_checker.sh file:

- **API_KEYS**: A list of your VirusTotal API keys. Add as many keys as needed to avoid exceeding the quota.

- **REQUESTS_PER_KEY**: The number of API requests allowed per key before rotating to the next one.

- **DELAY_BETWEEN_REQUESTS**: The delay (in seconds) between consecutive API requests to avoid hitting rate limits.

Example configuration:
```bash
API_KEYS=("key-1" "key-2" "key-3")
REQUESTS_PER_KEY=5
DELAY_BETWEEN_REQUESTS=20
```

## Notes
- API Key Limits:

    - Ensure your VirusTotal API keys are active and have sufficient quotas for your usage.
Rotate API keys using the built-in functionality to avoid hitting rate limits.

- Rate Limits:

    - Adjust DELAY_BETWEEN_REQUESTS in the configuration to respect API limits and prevent temporary bans.

- Security:

    - Keep your API keys private to prevent unauthorized usage.
    - Do not share or upload your API keys publicly.

