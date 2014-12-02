# Application Security

## OWASP 2013 Top 10

Each year, OWASP collate a list of the [top 10 of the most critical risks
facing organisations][top10].  It focusses on application security, and gives
details on both checking for vulnerabilities, as well as mitigation strategies.
The list is updated annually based on changing technology and prevalence of
attacks, and is organised based on this as well as risk factor.

OWASP also produce a series of [cheat sheets][cheat] which cover security best
practices for a wide variety of different development paradigms and languages.
Developers _and_ testers are required to read both the Top 10 list and
appropriate cheat sheets so they both understand the security issues that may
occur, and have the necessary tools to detect and avoid these issues within the
systems they build.

[top10]: https://www.owasp.org/index.php/Top_10_2013-Top_10
[cheat]: https://www.owasp.org/index.php/Cheat_Sheets


### 1. Injection Flaws

Injection flaws, such as SQL, OS, and LDAP injection occur when untrusted data
is sent to an interpreter as part of a command or query. The attacker’s hostile
data can trick the interpreter into executing unintended commands or accessing
data without proper authorization.

#### Checking for Vulnerabilities

* Audit code to check for any unvalidated input being sent to an external
  interpreter, including SQL, XML parsers, SMTP Headers, program arguments etc.

#### Prevention

* Use a safe API which provides a parameterised interface for making SQL calls,
  for example by using [`<cfqueryparam>`][cfqp] in ColdFusion code, or passing
  input as [hash values or with parameterisation][rorinj] when using
  ActiveRecord.
* If a parameterised API is not available, ensure all special characters are
  correctly escaped.
* Use whitelists for validating acceptable data, rather than blacklisting
  dangerous input.

[cfqp]: http://cfdocs.org/cfqueryparam
[rorinj]: http://guides.rubyonrails.org/security.html#sql-injection-countermeasures


### 2. Broken Authentication and Session Management

Application functions related to authentication and session management are
often not implemented correctly, allowing attackers to compromise passwords,
keys, or session tokens, or to exploit other implementation flaws to assume
other users identities.

#### Checking for Vulnerabilities

* Are session IDs exposed in the URL?
* Do session IDs timeout after a sensible period?
* Are sessions or authentication tokens properly invalidated during logout?
* Are session IDs rotated after successful login?
* Are passwords and other credentials sent over unencrypted connections?

#### Prevention

* Ensure all authentication and session management controls strive to meet all
  requirements defined in [OWASP's Application Security Verification
  Standard][owaspavs] areas V2 (Authentication) and V3 (Session Management).
* Avoid XSS flaws which could be used to steal session IDs.

[owaspavs]: https://www.owasp.org/index.php/ASVS


### 3. Cross-Site Scripting (XSS)

XSS flaws occur whenever an application takes untrusted data and sends it to a
web browser without proper validation or escaping. XSS allows attackers to
execute scripts in the victim’s browser which can hijack user sessions, deface
web sites, or redirect the user to malicious sites.

#### Checking for Vulnerabilities

* Is any user-supplied input stored server-side without being sanitised, and
  later redisplayed without correct escaping ("Stored XSS Attack")?
* Is any user-supplied input directly output back to the user without being
  sanitised or escaped in an error message, search result, or any other
  response that includes some or all of the input sent to the server as part of
  the request ("Reflected XSS Attack")?

#### Prevention

* Properly escape all untrusted data based on the context (HTML body, tag
  attribute, JavaScript, CSS or URL) that the data will be placed into.
* Perform whitelist validation on all untrusted input, covering length,
  characters, format and business rules.
* Consider using [Content Security Policy][csp] (CSP) as an additional XSS
  countermeasure.  [Twitter][csp-twit] and [GitHub][csp-gh] both have case
  studies, and HTML5 Rocks has a [good primer on CSP][csp-h5r].

[csp]: https://www.owasp.org/index.php/Content_Security_Policy
[csp-twit]: https://blog.twitter.com/2013/csp-to-the-rescue-leveraging-the-browser-for-security
[csp-gh]: https://github.com/blog/1477-content-security-policy
[csp-h5r]: http://www.html5rocks.com/en/tutorials/security/content-security-policy/


### 4. Insecure Direct Object References

A direct object reference occurs when a developer exposes a reference to an
internal implementation object, such as a file, directory, or database key.
Without an access control check or other protection, attackers can manipulate
these references to access unauthorized data.

#### Checking for Vulnerabilities

* Does the application fail to verify the user is authorised to access the
  exact resource they have requested?
* Is it possible to manipulate identifiers in a URL to access resources that
  should be inaccessible to the user?

#### Prevention

* Each use of a direct object reference from an untrusted source must include
  an access control check to ensure the user is authorised for the requested
  object.
* Use per-user or per-session indirect object references, requiring the
  application to map the per-user object references to the target reference.


### 5. Security Misconfiguration

Good security requires having a secure configuration defined and deployed for
the application, frameworks, application server, web server, database server,
and platform. Secure settings should be defined, implemented, and maintained,
as defaults are often insecure. Additionally, software should be kept up to
date.

#### Checking for Vulnerabilities

* Are any software libraries or server stack components out of date? This
  includes the operating system, web/application server, database,
  applications, and all code libraries.
* Are any unnecessary features enabled or installed (e.g. ports, services,
  pages, accounts, privileges)?
* Are default accounts and their passwords still enabled and unchanged?
* Does error handling reveal stack traces or other overly informative error
  messages to users?
* Are the security settings in your development frameworks and libraries not
  set to secure values?

#### Prevention

* Establish a repeatable hardening process that makes it fast and easy to
  deploy another environment that is properly locked down.  This should include
  ensuring that development, QA and production environments are all configured
  identically in an automated manner.
* Establish a process for keeping abreast of and deploying all new software
  updates and patches in a timely manner to each deployed environment,
  including all code libraries.
* Develop a strong application architecture that provides effective, secure
  separation between components.
* Consider running periodic scans and audits to detect future misconfigurations
  or missing patches.


### 6. Sensitive Data Exposure

Many web applications do not properly protect sensitive data, such as credit
cards, tax IDs, and authentication credentials. Attackers may steal or modify
such weakly protected data to conduct credit card fraud, identity theft, or
other crimes. Sensitive data deserves extra protection such as encryption at
rest or in transit, as well as special precautions when exchanged with the
browser.

#### Checking for Vulnerabilities

* Is any sensitive data stored in clear text long term, including backups of
  this data?
* Is any sensitive data transmitted in clear text, internally or externally?
* Are any old or weak cryptographic algorithms used?
* Are any browser security directives or headers missing when sensitive data is
  provided by or sent to the browser?

#### Prevention

* Encrypt all sensitive data at rest, and transmit in a manner that defends
  against any potential threats.
* Don't store sensitive data unnecessarily, and discard it as soon as possible.
* Ensure strong standard algorithms and strong keys are used, and proper key
  management is in place.
* Ensure passwords are stored with an algorithm specifically designed for
  password protection, such as [bcrypt][bcrypt], [PBKDF2][pbkdf2], or
  [scrypt][scrypt].
* Disable autocomplete on forms collecting sensitive data and disable caching
  for pages that contain sensitive data.

[bcrypt]: http://en.wikipedia.org/wiki/Bcrypt
[pbkdf2]: http://en.wikipedia.org/wiki/PBKDF2
[scrypt]: http://en.wikipedia.org/wiki/Scrypt


### 7. Missing Function-Level Access Control

Most web applications verify function level access rights before making that
functionality visible in the UI. However, applications need to perform the same
access control checks on the server when each function is accessed. If requests
are not verified, attackers will be able to forge requests in order to access
functionality without proper authorization.

#### Checking for Vulnerabilities

* Does the UI show navigation to unauthorised functions?
* Are server-side authentication or authorisation checks missing?
* Are server side checks done that solely rely on information provided by the
  attacker?

#### Prevention

* Think about the process for managing entitlements and ensure you can update
  and audit easily. Don’t hard code.
* The enforcement mechanism(s) should deny all access by default, requiring
  explicit grants to specific roles for access to every function.
* If the function is involved in a workflow, check to make sure the conditions
  are in the proper state to allow access.


### 8. Cross-Site Request Forgery (CSRF)

A CSRF attack forces a logged-on victim’s browser to send a forged HTTP
request, including the victim’s session cookie and any other automatically
included authentication information, to a vulnerable web application. This
allows the attacker to force the victim’s browser to generate requests the
vulnerable application thinks are legitimate requests from the victim.

#### Checking for Vulnerabilities

* Do any forms that invoke state-changing functions lack an unpredictable CSRF
  token?

#### Prevention

* Ensure that any links that result in a `GET` request to the server do not
  invoke any state-changing functions.
* Implement the [Synchronizer Token Pattern][stp], ideally using a token that
  is stored on the server-side as part of the user's session.
* Use unpredictable CSRF tokens that are, at a minimum, unique per user
  session.

[stp]: https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern


### 9. Using Components with Known Vulnerabilities

Components, such as libraries, frameworks, and other software modules, almost
always run with full privileges. If a vulnerable component is exploited, such
an attack can facilitate serious data loss or server takeover. Applications
using components with known vulnerabilities may undermine application defenses
and enable a range of possible attacks and impacts.

#### Checking for Vulnerabilities

* Are any components or library versions in use which have known
  vulnerabilities?
* Are any components or library versions in use which are no longer being
  supported or having security patches applied?

#### Prevention

* Identify all components and the versions of libraries being used, including
  all dependencies.
* Monitor the security of these components in public databases, project mailing
  lists, and security mailing lists, and appraise all applications against each
  security announcement.
* Employ a programme of regularly upgrading applications and libraries to
  ensure that future patches will be available for the version of the library
  in use.
* Establish security policies governing component use, such as requiring
  certain software development practices, passing security tests, and
  acceptable licenses.
* Where appropriate, consider adding security wrappers around components to
  disable unused functionality and/or secure weak or vulnerable aspects of the
  component.


### 10. Unvalidated Redirects and Forwards

Web applications frequently redirect and forward users to other pages and
websites, and use untrusted data to determine the destination pages. Without
proper validation, attackers can redirect victims to phishing or malware sites,
or use forwards to access unauthorized pages.

#### Checking for Vulnerabilities

* Review the code for all uses of redirect or forward. For each use, identify
  if the target URL is included in any parameter values. If so, if the target
  URL isn't validated against a whitelist, you are vulnerable.
* Spider the application to see if it generates any redirects (HTTP response
  codes 300-307, typically 302). Look at the parameters supplied prior to the
  redirect to see if they appear to be a target URL or a piece of such a URL.
  If so, change the URL target and observe whether the site redirects to the
  new target.

#### Prevention

* Avoid using redirects and forwards where possible.
* Don't involve user-supplied parameters in calculating destinations addresses.
* If destination parameters can't be avoided, ensure that the supplied value is
  valid and authorised to the user.


## Further Reading

* OWASP have produced a [Developer Guide][owasp-dev].  This is currently an
  ongoing project, with much content still to be completed but, when complete,
  will be a reference for developers on approaches for writing solid, safe and
  secure code. A PDF version of V2.0.1 of the document is [currently
  available][owasp-dev-pdf], although this version was published in 2005 and so
  is considered out of date, with much of the information now forming part of
  the Testing Guide (see below).
* OWASP have also created a freely-available [Testing Guide][owasp-test].  This
  document is up-to-date and covers a very wide variety of approaches, methods
  and tools for performing penetration testing against web-based software
  systems.
* [Zed Attack Proxy][owasp-zed] is an integrated penetration testing tool to
  assist in finding vulnerabilities in web applications. It provides automated
  scanners as well as a set of tools that allow security vulnerabilities to be
  discovered manually.

[owasp-dev]: https://www.owasp.org/index.php/OWASP_Guide_Project
[owasp-dev-pdf]: https://www.um.es/atica/documentos/OWASPGuide2.0.1.pdf
[owasp-test]: https://www.owasp.org/index.php/Category:OWASP_Testing_Project
[owasp-zed]: https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project
