# Information-Gathering-Using-OSINT-Tools
Information Gathering
Using OSINT Tools
WHOIS  •  theHarvester  •  nslookup  •  Google Dorking  •  Subfinder

### Table of Contents
1.	 Introduction
2.	 Project Objectives and Scope
3.	 Lab Setup and Tool Installation
4.	 Practical 1 — WHOIS Enumeration
5.	 Practical 2 — theHarvester
6.	 Practical 3 — nslookup
7.	 Practical 4 — Google Dorking
8.	 Practical 5 — Subfinder
9.	 Evidence Management and Observation Table
10.	 Methodology
11.	 Security, Ethics, and Limitations
12.	 Conclusion
13.	Appendix A — Suggested GitHub Structure
    
## 1. Introduction
- Information gathering is an early phase of a cybersecurity assessment. It involves collecting and reviewing publicly available information about a domain, organization, or its internet-facing presence. Open-Source Intelligence (OSINT) refers to information collected from public sources and analyzed for a specific purpose.
- This project demonstrates five beginner-friendly tools and techniques: WHOIS, theHarvester, nslookup, Google Dorking, and Subfinder. The practical work focuses on passive collection and documentation rather than exploitation or unauthorized access.

#### Project Details

| Item | Description |
|---|---|
| **Project Title** | Information Gathering in Cybersecurity Using OSINT Tools |
| **Platform** | Kali Linux |
| **Category** | Cybersecurity / Open-Source Intelligence (OSINT) |
| **Level** | Beginner |
| **Target** | A domain owned by the student or explicitly authorized for assessment |
| **Main Output** | Command outputs, observations, screenshots, and recommendations |


## 2. Project Objectives and Scope

-	Understand the role of OSINT and passive reconnaissance in cybersecurity.
-	Collect public domain registration information with WHOIS.
-	Use theHarvester to review publicly discoverable hostnames and other available information.
-	Query DNS records with nslookup.
-	Use Google search operators to locate publicly indexed pages and documents.
-	Discover subdomains with Subfinder using passive sources.
-	Document findings clearly and identify the limitations of each tool.

Scope and Authorization
- Use a domain you own or have explicit permission to assess. example.com is a reserved example domain that can be used to understand commands, but its results may be limited and will not represent a full real-world assessment. Do not attempt to log in to discovered services, exploit systems, bypass access controls, or access private information.

## 3. Lab Setup and Tool Installation
Open a terminal in Kali Linux and update the package lists:
sudo apt update
sudo apt upgrade
Install the initial tools:
sudo apt install whois dnsutils theharvester
Check tool help or availability. Some versions may not support the version flag; use -h when needed.
whois --version
theHarvester -h
nslookup -help
Install Subfinder
sudo apt install subfinder
subfinder -version
If Kali cannot find the package or the packaged version is unsuitable, follow the official ProjectDiscovery installation guide: https://docs.projectdiscovery.io/opensource/subfinder/install
Prepare an Evidence Folder
mkdir -p ~/information-gathering-project/evidence
cd ~/information-gathering-project
Replace example.com in the commands with your authorized assessment domain.

## 4. Practical — WHOIS Enumeration

A. Description
- WHOIS is a protocol and lookup service used to retrieve domain registration information. Depending on the registry and privacy settings, output may include the registrar, registration dates, name servers, status, and registrant details.

B. Commands to Perform

- whois example.com
- whois example.com | tee evidence/whois.txt

C. Observed Information
| Field / Item | What to Record |
|---|---|
| Registrar | Company managing the domain registration, if shown |
| Creation Date | Date the domain was registered, if provided |
| Expiry Date | Registration expiry date, if provided |
| Name Server | DNS servers associated with the domain |
| Domain Status | Registration status values |
| Registrant | Owner information only if publicly available |

D. Purpose
-   Understand domain registration details.
-	Identify the registrar and configured name servers.
-	Collect background information for an authorized assessment.
-	Record which information is hidden or unavailable.

E. Output Showing

<img width="1000" height="625" alt="image" src="https://github.com/user-attachments/assets/77199a0d-654f-44e7-9559-a84822001730" />
<img width="1000" height="625" alt="image" src="https://github.com/user-attachments/assets/18e7c52f-30af-427e-8706-d08f9499f925" />

## 5. Practical — theHarvester
A. Description
- theHarvester is an OSINT tool that gathers publicly available information about a domain using supported search engines and other public sources. Results depend on the selected source, network access, configuration, and source limits.

B. Commands to Perform
- theHarvester -h
- theHarvester -d example.com -b crtsh
- theHarvester -d example.com -b crtsh > evidence/theharvester.txt 2>&1
- The crt.sh source relates to public certificate-transparency data. If the selected source is unavailable in your installed version, consult the tool's help output and use a supported source.

C. Observed Information
| Field / Item | What to Record |
|---|---|
| **Emails** | Publicly discovered email addresses, if any |
| **Hosts** | Hostnames found by supported sources |
| **IP Addresses** | IP addresses associated with discovered hosts, if returned |
| **Sources** | Data sources queried by the tool |
| **Errors / Limits** | Unavailable sources, rate limits, or lookup errors |

D. Purpose
-	Learn how OSINT tools collect public information.
-	Review the public digital footprint of an authorized domain.
-	Understand that results can be incomplete and source-dependent.
-	Record useful findings without publishing sensitive details.

E. Output Showing

<img width="1000" height="625" alt="image" src="https://github.com/user-attachments/assets/78bd82d7-9caa-4695-ae8a-171558f6a63c" />
 <img width="1000" height="625" alt="image" src="https://github.com/user-attachments/assets/770a6ba9-a225-4098-b231-26339a13938d" />
 <img width="1000" height="625" alt="image" src="https://github.com/user-attachments/assets/5037561c-387d-412c-992d-6720d9996473" />
<img width="1000" height="625" alt="image" src="https://github.com/user-attachments/assets/31ec9818-e6e3-409c-a9f3-fc2ba5377b95" />

## 6. Practical — nslookup
A. Description
- nslookup is a DNS lookup utility. It queries the Domain Name System (DNS) to resolve domain names and retrieve DNS records.

B. Commands to Perform
- nslookup example.com
- nslookup -type=MX example.com
- nslookup -type=NS example.com
- nslookup -type=TXT example.com
- nslookup example.com > evidence/nslookup.txt
- Some records may not exist, and DNS responses can vary by resolver. An absent record is not automatically a security problem.

C. Observed Information
| DNS Record | Description |
|------------|-------------|
| A          | Maps a domain name to an IPv4 address. |
| AAAA       | Maps a domain name to an IPv6 address. |
| MX         | Mail Exchange records specify the mail servers responsible for receiving emails for a domain. |
| NS         | Name Server records identify the authoritative name servers for a domain. |
| TXT        | Text records store information such as SPF records and domain verification data. |

D. Purpose
-	Understand domain-to-IP resolution.
-	Identify publicly configured mail and name servers.
-	Examine DNS configuration for an authorized domain.
-	Learn how DNS supports web and email services.

E. Output Showing

<img width="1000" height="625" alt="image" src="https://github.com/user-attachments/assets/479b2bda-93a8-4af0-ba5e-5ff460ce54b1" />

## 7. Practical — Google Dorking
A. Description
- Google Dorking uses advanced search operators to locate publicly indexed information. It is a search technique rather than a separate software tool. Search results vary over time and may differ by region or account settings.

B. Search Queries to Perform
| Purpose | Example Query |
|---|---|
| Find indexed pages | `site:example.com` |
| Find public PDF documents | `site:example.com filetype:pdf` |
| Find public policy pages | `site:example.com (privacy OR security) policy` |
| Find public documentation | `site:example.com documentation` |

C. Observed Information
-	Publicly indexed web pages.
-	Public PDF files and documentation.
-	Public policy pages and descriptions of services.
-	Potentially outdated public content that the domain owner may wish to review.

D. Purpose
-	Learn advanced search techniques.
-	Understand how search engines index publicly accessible content.
-	Identify unnecessary public information exposure.
-	Recommend improvements to public information management.

E. Output Showing

<img width="1000" height="645" alt="image" src="https://github.com/user-attachments/assets/bc7cf910-8de5-41e7-9446-e7d6bf57ee1a" />
<img width="1000" height="680" alt="image" src="https://github.com/user-attachments/assets/6685c002-3556-40f2-9a96-4a91ca0bf0cc" />

## 8. Practical — Subfinder
A. Description
- Subfinder is a subdomain discovery tool from ProjectDiscovery. It primarily uses passive sources to identify subdomains associated with a domain. A discovered name does not prove that the host is currently active or vulnerable.

B. Commands to Perform
- subfinder -version
- subfinder -d example.com
- subfinder -d example.com -silent
- subfinder -d example.com -silent -o evidence/subdomains.txt
- wc -l evidence/subdomains.txt

C. Observed Information
| Field / Item | What to Record |
|---|---|
| Subdomains | Names associated with the target domain |
| Number of Results | Count of discovered subdomains |
| Source Coverage | Sources used for subdomain discovery |
| Limitations | Results depend on sources and configuration. No results do not prove that no subdomains exist. |

D. Purpose
-	Discover publicly known subdomains.
-	Understand the size of an organization's public namespace.
-	Help build an inventory for an authorized assessment.
-	Identify unexpected names for the domain owner to review.

E. Output Showing

<img width="1000" height="625" alt="image" src="https://github.com/user-attachments/assets/cf97c367-9bdf-4fa8-a8bc-39eb814f317a" />
<img width="1000" height="625" alt="image" src="https://github.com/user-attachments/assets/b9b66495-bef1-4e6a-9f54-d8ffb5905d90" />
<img width="1000" height="625" alt="image" src="https://github.com/user-attachments/assets/7f38958c-13b0-4a91-aa77-9d87812063a0" />

## 9. Evidence Management and Observation Table

| Tool | What to Record |
|---|---|
| WHOIS | Registrar, registration date, expiration date, name servers, and domain status |
| theHarvester | Discovered hosts, email addresses (if returned), and source errors |
| nslookup | IP addresses and DNS record results |
| Google Dorking | Search queries used and relevant publicly available results |
| Subfinder | Discovered subdomains and total number of subdomains found |

## 10. Methodology
1.	Define the scope and confirm permission to assess the selected domain.
2.	Collect public registration information using WHOIS.
3.	Use theHarvester to query supported public sources.
4.	Query address and DNS records with nslookup.
5.	Use safe Google search operators to review publicly indexed pages and documents.
6.	Use Subfinder for passive subdomain discovery.
7.	Save outputs, capture screenshots, and record observations.
8.	Review the findings, note limitations, and prepare recommendations for the domain owner.

## 11. Security, Ethics, and Limitations
-	Authorization: Assess only domains you own or have explicit permission to examine.
-	Passive does not mean risk-free: requests to third-party sources may be logged, and some tools may contact data providers.
-	No exploitation: This project is limited to information gathering; do not attempt to access accounts, exploit services, or bypass controls.
-	Protect evidence: Do not publish personal data, credentials, confidential details, or sensitive screenshots.
-	Interpret carefully: Results may be outdated, incomplete, rate-limited, or affected by DNS and search-engine behavior.
-	Respect laws, site terms, and organizational policies.

## 12. Conclusion
- This project demonstrates five information-gathering techniques in cybersecurity: WHOIS Enumeration, theHarvester, nslookup, Google Dorking, and Subfinder. These tools help security practitioners review public domain registration details, DNS records, indexed pages, and known subdomains.
- The practical builds familiarity with Open-Source Intelligence (OSINT), passive reconnaissance, evidence collection, and clear reporting. Findings should be interpreted cautiously, documented accurately, and used only within an authorized scope.
