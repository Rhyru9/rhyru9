<p align="center">
  <img
    src="https://github.com/RHYru9/rhyru9/blob/main/R.gif"
    alt="banner"
    width="100%"
  />
</p>

<h1 align="center">Rey</h1>

<p align="center">
  Bug hunter. Most of the work is in a proxy: replay a request, change one
  assumption, see what the server still allows.
</p>

<p align="center">
  <img src="https://readme-typing-svg.demolab.com/?font=Fira+Code&weight=500&size=16&duration=3400&pause=1200&color=38BDF8&center=true&vCenter=true&width=640&lines=Access+control%2C+injection%2C+misconfig;Authn+holds.+Authz+is+usually+the+gap;Small+tools+for+recon+I+keep+repeating" />
</p>

<p align="center">
  <a href="https://rhyru9.xyz">
    <img src="https://img.shields.io/badge/Portfolio-rhyru9.xyz-38BDF8?style=flat-square&logo=firefox&logoColor=white&labelColor=0D1117" />
  </a>
  <a href="https://github.com/RHYru9">
    <img src="https://img.shields.io/badge/GitHub-RHYru9-38BDF8?style=flat-square&logo=github&logoColor=white&labelColor=0D1117" />
  </a>
  <img src="https://komarev.com/ghpvc/?username=rhyru9&label=Views&color=38BDF8&style=flat-square" />
</p>

---

## about

I look at how web apps and APIs fail in production — mostly through bug bounty
programs and CTFs.

The useful bugs sit between what the UI claims and what the HTTP actually does.
Between reports I write small tools, because recon eats the night if I let it.
If I’ve done a step twice by hand, it tends to become a script.

## where the time goes

Web is home base. Access control first: IDOR, BOLA, hidden admin routes, CORS
that reflects origin and still sends cookies. Then identity — session reuse
after logout, reset-link poisoning, MFA that only lives in the UI, JWT
`alg` / `kid` / `none`, OAuth `redirect_uri` treated as decoration.

Injection sits next to that. SQLi, NoSQLi, XSS (reflected, stored, DOM), SSTI,
command injection, XXE, header/CRLF. CSRF on state-changing requests that never
got a token. Uploads that trust `Content-Type`. Path traversal and LFI. Open
redirects that become XSS or token theft two steps later. Request smuggling and
cache deception when the proxies disagree. Prototype pollution and DOM
clobbering on heavy frontends. Business logic is slower — you have to understand
how the product makes money before you can make it spend money wrong.

APIs hide the same mistakes. REST and GraphQL (introspection left on, batching,
depth, a nested field that skipped the check the REST route had). JWTs trusted
too hard. OAuth as “redirect and hope.” A field that wasn’t in the docs until I
sent it anyway. Mass assignment, parameter pollution, SSRF, races.
Deserialization — pickle, Java, PHP `unserialize` — and webhooks that were
never verified.

Cloud and supply chain when the app is only half the target. Private buckets
that aren’t. Default creds, debug, directory listing, stack traces. Forgotten
subdomains. CI printing a token into a log. Outdated deps with a CVE that
already has a name. Exception handlers that leak or fail open.

OWASP Top 10 (2025) is the starting map. Named version is in the index.

<details>
<summary><code>index</code></summary>

<br>

**access control** *(A01)* — IDOR, BOLA, privilege escalation, forced browsing, missing function-level checks, CORS with credentials, CSRF, cached pages that belong to someone else

**authn / session** *(A07)* — session fixation, reuse after logout, credential stuffing, weak reset, Host-header reset poisoning, MFA bypass, JWT `none` / alg confusion / `kid`, OAuth `redirect_uri` / missing `state`, cookie flags (`Secure`, `HttpOnly`, `SameSite`)

**injection** *(A05)* — SQLi, NoSQLi, XSS (reflected / stored / DOM), SSTI, OS command injection, XXE, LDAP, header injection, CRLF

**files, fetch, proxy** — path traversal, LFI / RFI, unrestricted & polyglot uploads, SSRF, open redirect, request smuggling / desync, Host header, web cache poisoning & cache deception

**crypto** *(A04)* — secrets in transit or in the page, weak hashing, JWT that isn’t signed like it claims, home-rolled crypto

**misconfig** *(A02)* — default creds, debug / stack traces, directory listing, fat CORS, exposed `.git` / backups / admin, cloud bucket & IAM, subdomain takeover

**supply chain & integrity** *(A03, A08)* — outdated components, known CVEs, CI/CD secrets in logs, unsigned webhooks, insecure deserialization, prototype pollution, dependency confusion

**design & logic** *(A06)* — race / TOCTOU, missing rate limit, mass assignment, parameter pollution, business rules that trust the client, GraphQL depth/batching

**logging & exceptions** *(A09, A10)* — alerts that never fire, logs that store secrets, handlers that leak internals, fail-open on timeout, 4xx/5xx that skip authz

**client-side** — DOM clobbering, postMessage origin skips, clickjacking, tabnabbing, CSS injection, WebSocket with no auth

</details>

## on the desk

Burp Suite for the request work. Nmap and Yakit for ports and assets. ZAP when
I want a second proxy.

Python when the tool needs to exist today. Go when it has to stay fast. PHP
when the target is PHP and matching the language makes the bug easier to see.

<p>
  <img src="https://img.shields.io/badge/Burp_Suite-38BDF8?style=flat-square&labelColor=0D1117" />
  <img src="https://img.shields.io/badge/OWASP_ZAP-38BDF8?style=flat-square&labelColor=0D1117" />
  <img src="https://img.shields.io/badge/Nmap-38BDF8?style=flat-square&labelColor=0D1117" />
  <img src="https://img.shields.io/badge/Yakit-38BDF8?style=flat-square&labelColor=0D1117" />
  &nbsp;
  <img src="https://img.shields.io/badge/Python-38BDF8?style=flat-square&logo=python&logoColor=white&labelColor=0D1117" />
  <img src="https://img.shields.io/badge/Go-38BDF8?style=flat-square&logo=go&logoColor=white&labelColor=0D1117" />
  <img src="https://img.shields.io/badge/PHP-38BDF8?style=flat-square&logo=php&logoColor=white&labelColor=0D1117" />
</p>

## built

The recon steps that kept repeating turned into these.

[GitSD](https://github.com/rhyru9/GitSD) (Go) walks domains and subdomains for
exposed `.git`, then reads what's inside — credentials, keys, source.

[KODOK](https://github.com/rhyru9/kodok) (Go) is the JavaScript pass: hidden API
endpoints, JS dependency chains, secrets with entropy so the noise drops.

[PathKit](https://github.com/Rhyru9/PathKit) (Python, no deps) sits later in
triage. It splits human slugs from UUID / hash / timestamp / base64 junk so
recon and IDOR don't treat every path the same.

## github

<p align="center">
  <img height="165" src="https://github-stats-extended.vercel.app/api?username=RHYru9&show_icons=true&include_all_commits=true&hide_rank=true&hide_border=true&bg_color=16181D&title_color=38BDF8&text_color=c9d1d9&icon_color=38BDF8" />
  <img height="165" src="https://github-stats-extended.vercel.app/api/top-langs/?username=RHYru9&layout=compact&hide_border=true&bg_color=16181D&title_color=38BDF8&text_color=c9d1d9" />
</p>
<p align="center">
  <img height="165" src="https://streak-stats.demolab.com/?user=RHYru9&theme=dark&hide_border=true&background=16181D&ring=38BDF8&fire=38BDF8&currStreakNum=ffffff&sideNums=c9d1d9&currStreakLabel=38BDF8&sideLabels=c9d1d9&dates=8b8fa3" />
</p>

## right now

Bug bounty on web and API. PathKit is the one still moving. GitSD and KODOK
when a recon pass needs them. Notes from testing, when they’re worth keeping.

---

<p align="center">
  <a href="https://rhyru9.xyz">Website</a>
  &nbsp;&nbsp;·&nbsp;&nbsp;
  <a href="https://github.com/RHYru9">GitHub</a>
  &nbsp;&nbsp;·&nbsp;&nbsp;
  <a href="https://hackerone.com/rhyru9">HackerOne</a>
  &nbsp;&nbsp;·&nbsp;&nbsp;
  <a href="https://yeswehack.com/hunters/rhyru9">YesWeHack</a>
</p>
