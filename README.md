<h1>Web Content Discovery (TryHackMe Notes)</h1>
<P>Content Discovery is the process of identifying hidden or unlinked resources on a web server. These resources may not be directly visible to users but can provide useful information to attackers or testers. Content can include files, media, backup archives, configuration files, admin panels, or outdated versions of a website.

While some content is intentionally exposed, other resources may be inadvertently left accessible due to misconfigurations or oversight.</P>
</br>
<h2>🔹 What is Considered Content?</h2>
- Files (e.g., .php, .html, .bak)

- Videos, images, and media

- Admin or internal portals

- Configuration and log files

- Development/testing versions of a website

- Framework leftovers (e.g., default pages)

These are not always linked from the main page or indexed but may still be accessible directly.
</br>

<h3>🔹 Methods of Content Discovery</h3>
There are three main approaches:

- Manual Discovery

  - Guessing common paths like /admin, /login, /dashboard

  - Inspecting JavaScript and HTML source for hidden endpoints

  - Reviewing exposed files like robots.txt or sitemap.xml

- Automated Discovery

  - Using tools like ffuf, dirb, or gobuster with large wordlists to brute-force directories and filenames.

- OSINT (Open-Source Intelligence)

  - Using public data sources (e.g., Google Dorking, GitHub, Archive.org) to gather information about potential content.
</br>
<h2>🔹 Key Artifacts for Manual Discovery</h2>
1. robots.txt
- Used to instruct search engines on what content should or shouldn't be indexed.
- May reveal sensitive or hidden URLs accidentally disallowed (e.g., /secret/, /backup/).
</br>

2. favicon.ico
- The small icon shown in browser tabs.
- Often left unchanged after using a web framework (e.g., Laravel, WordPress).
- Can help identify the technology stack in use if not replaced by developers.
</br>

3. sitemap.xml
- Lists all URLs the site owner wants indexed.
- Can include hidden, older, or rarely accessed pages.
- Often more detailed than the main navigation.
</br>

4. HTTP Headers
- When a request is made to a web server, the response contains HTTP headers.
- These may reveal information like:
- Server software (Apache, nginx)
- Technologies in use (PHP, ASP.NET)
- Frameworks and versions
</br>

<h3>🔹 Identifying the Framework Stack</h3>
- Inspect the HTML source for comments, copyrights, or meta tags.
- Use favicon or URL patterns as clues.
- Once identified, research the framework for common default paths or security issues.
</br>

<h2>🔹 OSINT Techniques</h2>
1. Google Dorking
- Leverages advanced Google search filters to discover hidden pages:
- site:tryhackme.com
- inurl:admin
- filetype:pdf
- intitle:login
</br>

2. Wappalyzer
- A browser extension or online tool to identify:
  - CMS (e.g., WordPress, Joomla)
  - Frameworks (e.g., React, Angular)
  - Analytics, payment systems, and more
- Often reveals technology versions in use.
</br>

3. Wayback Machine
- archive.org/web
- Lets you view historical snapshots of a website.
- Useful for discovering pages that were removed or updated but still functional.
</br>
4. GitHub Search
- Git is a version control system, and GitHub hosts public repositories.
- By searching for a company or domain, you may find:
  - Source code
  - Configuration files
  - Hardcoded credentials
- Public repos may contain leaked or forgotten files.
</br>
5. Amazon S3 Buckets
- S3 is a cloud storage service used to host files or static websites.
- If misconfigured, buckets may be:
  - Publicly readable
  - Publicly writable
  - Indexed by search engines
- Common URL structure:
  https://{bucket-name}.s3.amazonaws.com
- Bucket names often follow patterns like:

  - {company}-assets

  - {company}-public

  - {company}-backup
</br>
<h2>🔹 Automated Discovery</h2>
Automated tools help discover content quickly by sending large volumes of requests using <b>wordlists</b>.

<h3>What are Wordlists?</h3>
- Text files containing a list of commonly used file and directory names.
- Example: SecLists by Daniel Miessler.

  Different wordlists serve different purposes:

  - Password cracking

  - Directory brute-forcing

  - Fuzzing forms or APIs

<h2>Tools & Usage Examples</h2>
✅ FFUF

    ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -u http://example.com/FUZZ
 </br>   
✅ Dirb

    dirb http://example.com /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
</br>
✅ Gobuster

    gobuster dir --url http://example.com -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
</br>

<h2>✅ Summary</h2>
<p><i>Content discovery allows security testers to uncover hidden files and endpoints that may be useful for further exploitation. Combining manual techniques, automated tools, and OSINT strategies gives a thorough and effective reconnaissance process.</i></p>
