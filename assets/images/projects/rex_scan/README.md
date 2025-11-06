# REX SCAN

A comprehensive, automated network reconnaissance and vulnerability scanner designed for penetration testers and security professionals.

## Overview

REX SCAN is a powerful pentesting toolkit that orchestrates multiple security tools to perform thorough network reconnaissance, service enumeration, vulnerability detection, and exploitation research. It automates the workflow of professional security assessments while producing detailed, actionable reports in multiple formats.

Built with modularity and extensibility in mind, REX SCAN integrates seamlessly with industry-standard tools while providing intelligent correlation of findings, automated credential testing, and advanced web application analysis.

## Key Capabilities

### Network Reconnaissance
- **Automated Port Scanning**: Intelligent nmap integration with customizable timing profiles
- **Service Detection**: Deep service fingerprinting and version detection
- **Multi-Target Support**: Scan single IPs, CIDR ranges, IP ranges, or target lists
- **Flexible Scan Profiles**: Pre-configured profiles from stealth to aggressive

### Enumeration & Analysis
- **DNS Enumeration**: Subdomain discovery, zone transfer detection, DNS record analysis
- **Directory Enumeration**: Web path discovery with intelligent wordlist management
- **SSH Analysis**: Algorithm enumeration, weak cipher detection, version analysis
- **SMB Analysis**: Share enumeration, null session detection, version fingerprinting
- **Advanced Web Analysis**: SSL/TLS analysis, security headers, CMS detection, technology fingerprinting

### Vulnerability Assessment
- **CVE Correlation**: Automated CVE lookup via NIST NVD API based on detected services and versions
- **CVSS Scoring**: Real-time CVSS v3.1/v2.0 scores from National Vulnerability Database
- **Exploit Database Integration**: Searchsploit integration for exploit availability
- **Severity Classification**: CRITICAL, HIGH, MEDIUM, LOW categorization with exploit verification
- **Credential Testing**: Conservative testing for default credentials and anonymous access

### Automation & Reliability
- **Resume Capability**: Interrupt and resume long-running scans without data loss
- **State Management**: Automatic scan state tracking and recovery
- **Rate Limiting**: Configurable request throttling to avoid detection
- **Error Handling**: Graceful error recovery with detailed logging

### Reporting
- **Multi-Format Output**: Text, JSON, and HTML reports
- **Interactive Dashboards**: Rich HTML reports with charts and visualizations
- **Command Transparency**: Comprehensive tracking of all external commands executed (nmap, searchsploit, gobuster, dnsenum, etc.)
- **Method Indicators**: Reports show which tool/method was used for each module (e.g., "Method: gobuster" vs "Method: python")
- **Screenshot Capture**: Automated web service screenshot capture
- **Scan Comparison**: Diff two scans to identify new vulnerabilities or changes

### Command Tracking & Transparency

REX SCAN tracks **every external command** executed during a scan:

- **Comprehensive Logging**: All tools tracked (nmap, searchsploit, gobuster, dirb, dnsenum, ssh-audit, smbclient, rpcclient)
- **Full Output Capture**: Complete stdout/stderr for each command
- **Execution Metrics**: Command duration, exit codes, timestamps
- **Security**: Passwords automatically masked in logged commands
- **Separate Report**: Dedicated `commands.html` with all command details
- **Module Status**: Main report clearly shows which modules ran and with what tools

This transparency is crucial for:
- Reproducing scans manually
- Understanding tool behavior
- Audit trails and documentation
- Troubleshooting and debugging

## Tools Used

REX SCAN uses a **dual-tool architecture** with intelligent fallback - external tools are preferred for performance but Python fallbacks ensure core functionality always works.

### Required (Core)
- **nmap**: Network scanning and service detection (REQUIRED - no fallback)
- **Python 3.8+**: Core runtime environment (REQUIRED)

### Optional External Tools (Recommended)
- **searchsploit** (exploit-db): Exploit database queries - highly recommended
- **gobuster**: High-performance directory enumeration → falls back to Python requests
- **dnsenum**: Advanced DNS enumeration → falls back to dnspython library
- **dirb**: Alternative directory brute-forcing → falls back to Python requests
- **ssh-audit**: SSH security auditing → falls back to banner grabbing
- **smbclient/rpcclient**: SMB enumeration → limited fallback
- **Playwright**: Web service screenshot capture (optional feature)

### Python Fallbacks (Always Available)
- **dnspython**: DNS queries and basic subdomain enumeration
- **requests**: HTTP-based directory enumeration with wordlists
- **Built-in socket**: SSH banner grabbing, basic service detection

**Key Point**: REX SCAN works out-of-the-box with just Python and nmap. External tools enhance performance and capabilities but are not mandatory.

### Python Libraries
- **dnspython**: DNS resolution and enumeration
- **requests/httpx**: HTTP client for web enumeration and NVD API queries
- **Jinja2**: Report templating engine
- **matplotlib/plotly**: Chart generation and visualization
- **colorama**: Terminal output formatting
- **tqdm**: Progress bars and status display

### External APIs
- **NIST NVD API**: Real-time CVE data and CVSS scoring (https://services.nvd.nist.gov/rest/json/cves/2.0)
- **Cloudflare DNS-over-HTTPS**: Optional privacy-enhanced DNS queries

## Installation

### Prerequisites

```bash
# Ensure you have Python 3.8 or higher
python3 --version

# Install nmap (required)
# macOS:
brew install nmap

# Ubuntu/Debian:
sudo apt-get install nmap

# Fedora/RHEL:
sudo yum install nmap
```

### Quick Install

```bash
# Clone the repository
git clone https://github.com/rexmirak/rex-scan.git
cd rex-scan

# Create virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install Python dependencies
pip install -r requirements.txt

# Install optional external tools (recommended for best performance)
bash install_tools.sh

# Install Playwright browsers for screenshots (optional)
playwright install chromium

# Verify installation
python -m rex_scan --help
```

**Note**: REX SCAN works immediately after `pip install -r requirements.txt` with just nmap. The `install_tools.sh` script adds optional performance enhancements.

### Optional Tools Installation

REX SCAN works perfectly without these tools (Python fallbacks are used), but external tools provide better performance:

```bash
# Automated installation (recommended)
bash install_tools.sh

# Manual installation by OS:
# macOS
brew install nmap exploitdb gobuster
# Note: dnsenum/dirb not available on macOS - Python fallbacks work perfectly

# Ubuntu/Debian (most tools available)
sudo apt-get install nmap exploitdb gobuster dnsenum dirb

# Fedora/RHEL
sudo yum install nmap exploitdb gobuster
```

**Tool Availability by OS:**
| Tool | macOS | Linux | Fallback |
|------|-------|-------|----------|
| nmap | ✓ Homebrew | ✓ Package manager | None (required) |
| searchsploit | ✓ Homebrew | ✓ Package manager | None (recommended) |
| gobuster | ✓ Homebrew | ✓ Package manager | Python requests |
| dnsenum | ✗ Not available | ✓ Package manager | dnspython |
| dirb | ✗ Not available | ✓ Package manager | Python requests |
| ssh-audit | ✗ Not in brew | ✗ Manual install | Banner grabbing |
| smbclient | ✗ Not in brew | ✓ Usually pre-installed | Limited |

## Usage

### Basic Syntax

```bash
python -m rex_scan --target <TARGET> [OPTIONS] --consent
```

### Scan Profiles

REX SCAN includes pre-configured profiles optimized for different scenarios:

| Profile | Timing | Ports | Use Case |
|---------|--------|-------|----------|
| `stealth` | T1 (Slow) | Top 1000 | Evade IDS/IPS detection |
| `balanced` | T3 (Normal) | Top 1000 | Default safe scanning |
| `aggressive` | T4 (Fast) | Top 1000 | Time-sensitive assessments |
| `quick` | T5 (Insane) | Top 1000 | Rapid reconnaissance |
| `full` | T3 (Normal) | All 65535 | Comprehensive assessment |
| `custom` | User-defined | Custom | Full control with --nmap-flags |

### Command Line Options

```
Target Specification:
  --target IP|HOSTNAME|CIDR    Single target or CIDR range
  --targets @FILE              File with targets (one per line)
  --ports RANGE                Port range (default: 1-1000)

Scan Configuration:
  --profile PROFILE            Scan profile (stealth|balanced|aggressive|quick|full|custom)
  --nmap-flags FLAGS           Custom nmap flags (requires --profile custom)
  --rate-limit N               Max requests per second (default: 10)
  --delay N                    Delay between requests in seconds

Enumeration:
  --dns-wordlist FILE|auto     DNS subdomain wordlist
  --dir-wordlist FILE|auto     Directory enumeration wordlist
  --advanced-web               Enable advanced web analysis
  --screenshots                Capture web service screenshots

Vulnerability Assessment:
  --no-vulns                   Disable vulnerability correlation
  --no-creds                   Disable credential testing

Output:
  --output DIR                 Output directory (default: ~/Desktop/rex_scan_<timestamp>)
  --no-charts                  Disable chart generation
  --quiet                      Minimal output

Control:
  --consent                    Skip authorization prompt
  --yes                        Answer yes to all prompts
  --resume DIR                 Resume interrupted scan
  --diff SCAN1 SCAN2           Compare two scans
```

## Quick Examples

### Single Host Scan

```bash
# Basic scan with default settings
python -m rex_scan --target 192.168.1.100 --consent

# Stealth scan (slow, evasive)
python -m rex_scan --target 192.168.1.100 --profile stealth --consent

# Quick scan (fast reconnaissance)
python -m rex_scan --target 192.168.1.100 --profile quick --consent
```

### Network Range Scanning

```bash
# Scan entire subnet
python -m rex_scan --target 192.168.1.0/24 --consent

# Scan IP range
python -m rex_scan --target 192.168.1.1-50 --consent

# Multiple targets from file
echo "192.168.1.100" > targets.txt
echo "192.168.1.200" >> targets.txt
python -m rex_scan --targets @targets.txt --consent
```

### Comprehensive Assessment

```bash
# Full scan with all enumeration features
python -m rex_scan \
  --target 192.168.1.100 \
  --profile full \
  --dns-wordlist auto \
  --dir-wordlist auto \
  --advanced-web \
  --screenshots \
  --consent \
  --output ~/security-assessment
```

### Custom Nmap Scan

```bash
# Advanced nmap options
python -m rex_scan \
  --target 192.168.1.100 \
  --profile custom \
  --nmap-flags "-sS -sV -O -A -T4 -p-" \
  --consent
```

### Resume Interrupted Scan

```bash
# If scan was interrupted (Ctrl+C)
python -m rex_scan --resume ~/Desktop/rex_scan_2025-11-04_123456
```

### Scan Comparison

```bash
# Compare two scans to identify changes
python -m rex_scan \
  --diff ~/scans/baseline/REX_REPORTS/report.json \
        ~/scans/current/REX_REPORTS/report.json
```

## Where to Find Results

Each scan creates a timestamped directory with organized output:

```
~/Desktop/rex_scan_2025-11-04_123456/
│
├── REX_REPORTS/                    # Final consolidated reports
│   ├── report.txt                  # Human-readable text report
│   ├── report.json                 # Machine-parsable JSON output
│   ├── report.html                 # Interactive HTML dashboard (open in browser)
│   ├── commands.html               # Command transparency report (all executed commands)
│   └── charts/                     # Visualization charts
│       ├── port_distribution.png
│       ├── credential_success.png
│       └── network_topology.html
│
├── INDIVIDUAL/                     # Raw tool outputs and structured data
│   ├── nmap.xml                    # Raw nmap XML output
│   ├── nmap_parsed.json            # Structured nmap data
│   ├── dns_enum.json               # DNS enumeration results (method tracked)
│   ├── dir_enum.json               # Directory scan results (method tracked)
│   ├── ssh_enum.json               # SSH analysis results
│   ├── smb_enum.json               # SMB analysis results
│   ├── advanced_web_enum.json      # Web analysis results
│   ├── searchsploit.txt            # Exploit database results
│   ├── vulnerabilities.json        # CVE correlations
│   ├── credentials.json            # Credential test results
│   └── screenshots/                # Web service screenshots
│       ├── http_192.168.1.100_80.png
│       └── https_192.168.1.100_443.png
```

### Viewing Results

**HTML Report (Recommended):**
```bash
# Open the interactive HTML report in your browser
open ~/Desktop/rex_scan_*/REX_REPORTS/report.html

# View command transparency report (all executed commands with raw output)
open ~/Desktop/rex_scan_*/REX_REPORTS/commands.html
```

The HTML reports now include:
- **report.html**: Complete scan results with module status indicators showing which tools were used (e.g., "Method: dnspython", "Method: gobuster")
- **commands.html**: Full transparency of every external command executed, with stdout/stderr, exit codes, and execution time

**Text Report:**
```bash
# View human-readable text report
cat ~/Desktop/rex_scan_*/REX_REPORTS/report.txt
```

**JSON Report (For Integration):**
```bash
# Parse JSON output with jq
cat ~/Desktop/rex_scan_*/REX_REPORTS/report.json | jq .
```

## Security & Ethics

**IMPORTANT**: REX SCAN is designed for authorized security testing only.

- ⚠️ **Authorization Required**: Only scan systems you own or have explicit written permission to test
- ⚠️ **Legal Compliance**: Unauthorized scanning may violate laws in your jurisdiction
- ⚠️ **Network Impact**: Aggressive scans can disrupt services or trigger alerts
- ⚠️ **Data Protection**: Scan results may contain sensitive information

### Best Practices

- Always obtain written authorization before testing
- Use appropriate scan profiles (`--profile stealth` for sensitive environments)
- Respect rate limits and consider `--delay` for fragile systems
- Secure scan outputs and reports appropriately
- Review and comply with applicable laws and regulations

## Contributing

We welcome contributions from the security community! Whether you're fixing bugs, adding features, improving documentation, or sharing ideas, your help makes REX SCAN better for everyone.

### How to Contribute

1. **Fork the Repository**
   ```bash
   git clone https://github.com/rexmirak/rex-scan.git
   cd rex-scan
   ```

2. **Create a Feature Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make Your Changes**
   - Write clean, documented code
   - Follow existing code style and conventions
   - Add tests for new functionality

4. **Test Your Changes**

5. **Submit a Pull Request**
   - Provide a clear description of changes
   - Reference any related issues
   - Ensure all tests pass

### Areas for Contribution

- **New Enumeration Modules**: Add support for additional services (RDP, VNC, etc.)
- **Enhanced Reporting**: Improve report templates and visualizations
- **Performance Optimization**: Speed improvements and resource management
- **Documentation**: Improve guides, examples, and API documentation
- **Testing**: Expand test coverage and add new test cases
- **Bug Fixes**: Identify and fix issues

### Development Setup

```bash
# Install development dependencies
pip install -r requirements.txt

# Install pre-commit hooks (optional)
# pip install pre-commit
# pre-commit install

```

### Reporting Issues

Found a bug or have a feature request? [Open an issue](https://github.com/rexmirak/rex-scan/issues) with:
- Clear description of the problem/feature
- Steps to reproduce (for bugs)
- Expected vs actual behavior
- Environment details (OS, Python version, tool versions)

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) file for details.

## Disclaimer

REX SCAN is provided for educational and authorized security testing purposes only. The authors and contributors are not responsible for misuse or damage caused by this tool. Users are solely responsible for ensuring they have proper authorization before conducting any security assessments.

## Acknowledgments

- **nmap** by Gordon Lyon (Fyodor) - The foundation of network scanning
- **exploit-db** by Offensive Security - Vulnerability and exploit database
- **gobuster** by OJ Reeves - High-performance directory enumeration
- **dnsenum** - Advanced DNS enumeration capabilities
- **dirb** - Web content scanner
- The open-source security community for continued innovation

---

For questions, issues, or contributions, visit our [GitHub repository](https://github.com/rexmirak/rex-scan).
