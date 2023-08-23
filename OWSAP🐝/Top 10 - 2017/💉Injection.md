

## What is Injection?
SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. It generally allows an attacker to view data that they are not normally able to retrieve. This might include data belonging to other users, or any other data that the application itself is able to access. In many cases, an attacker can modify or delete this data, causing persistent changes to the application's content or behavior.

In some situations, an attacker can escalate a SQL injection attack to compromise the underlying server or other back-end infrastructure, or perform a denial-of-service attack.

![[Pasted image 20230823164812.png]]
[Injection Flaws](https://owasp.org/www-community/Injection_Flaws)

## When is the Application Vulnerable to Injection Attack?

An application is vulnerable to attack when: 
- User-supplied data is not validated, filtered, or sanitized by the application. 
- Dynamic queries or non-parameterized calls without context aware escaping are used directly in the interpreter. 
- Hostile data is used within object-relational mapping (ORM) search parameters to extract additional, sensitive records. 
- Hostile data is directly used or concatenated, such that the SQL or command contains both structure and hostile data in dynamic queries, commands, or stored procedures. 

Some of the more common injections are SQL, NoSQL, OS command, Object Relational Mapping (ORM), LDAP, and Expression Language (EL) or Object Graph Navigation Library (OGNL) injection. The concept is identical among all interpreters. 

Source code review is the best method of detecting if applications are vulnerable to injections, closely followed by thorough automated testing of all parameters, headers, URL, cookies, JSON, SOAP, and XML data inputs. Organizations can include static source
[SAST](https://owasp.org/www-community/Source_Code_Analysis_Tools)and dynamic application test [DAST](https://owasp.org/www-community/Vulnerability_Scanning_Tools) tools into the CI/CD pipeline to identify newly introduced injection flaws prior to production deployment.

The majority of SQL injection vulnerabilities can be found quickly and reliably using Burp Suite's [web vulnerability scanner](https://portswigger.net/burp/vulnerability-scanner).

SQL injection can be detected manually by using a systematic set of tests against every entry point in the application. This typically involves:

- Submitting the single quote character `'` and looking for errors or other anomalies.
- Submitting some SQL-specific syntax that evaluates to the base (original) value of the entry point, and to a different value, and looking for systematic differences in the resulting application responses.
- Submitting Boolean conditions such as `OR 1=1` and `OR 1=2`, and looking for differences in the application's responses.
- Submitting payloads designed to trigger time delays when executed within a SQL query, and looking for differences in the time taken to respond.
- Submitting [OAST](https://portswigger.net/burp/application-security-testing/oast) payloads designed to trigger an out-of-band network interaction when executed within a SQL query, and monitoring for any resulting interactions.


## Example Attack Scenarios

Scenario #1: 
An application uses untrusted data in the construction of the following vulnerable SQL call:

```SQL
String query = "SELECT * FROM accounts WHERE custID='" + request.getParameter("id") + "'";
```

Scenario #2: Similarly, an application’s blind trust in frameworks may result in queries that are still vulnerable, (e.g. Hibernate Query Language (HQL)):

```SQL
Query HQLQuery = session.createQuery("FROM accounts WHERE custID='" + request.getParameter("id") + "'");
```

In both cases, the attacker modifies the ‘id’ parameter value in their browser to send: ' or '1'='1.

	For example: http://example.com/app/accountView?id=' or '1'='1 

This changes the meaning of both queries to return all the records from the accounts table. More dangerous attacks could modify or delete data, or even invoke stored procedures.
## How to Prevent

Preventing injection requires keeping data separate from commands and queries. 

- The preferred option is to use a safe API, which avoids the use of the interpreter entirely or provides a parameterized interface, or migrate to use Object Relational Mapping Tools (ORMs). 

**Note:** Even when parameterized, stored procedures can still introduce SQL injection if PL/SQL or T-SQL concatenates queries and data, or executes hostile data with EXECUTE IMMEDIATE or exec(). 

- Use positive or "whitelist" server-side input validation. This is not a complete defense as many applications require special characters, such as text areas or APIs for mobile applications. 
- For any residual dynamic queries, escape special characters using the specific escape syntax for that interpreter. 

**Note:** SQL structure such as table names, column names, and so on cannot be escaped, and thus user-supplied structure names are dangerous. This is a common issue in report-writing software. 

- Use LIMIT and other SQL controls within queries to prevent mass disclosure of records in case of SQL injection.

### References
	OWASP 
	• OWASP Proactive Controls: Parameterize Queries 
	• OWASP ASVS: V5 Input Validation and Encoding 
	• OWASP Testing Guide: SQL Injection, Command Injection, ORM injection 
	• OWASP Cheat Sheet: Injection Prevention 
	• OWASP Cheat Sheet: SQL Injection Prevention 
	• OWASP Cheat Sheet: Injection Prevention in Java 
	• OWASP Cheat Sheet: Query Parameterization 
	• OWASP Automated Threats to Web Applications – OAT-014 
	
	External 
	• CWE-77: Command Injection 
	• CWE-89: SQL Injection 
	• CWE-564: Hibernate Injection 
	• CWE-917: Expression Language Injection 
	• PortSwigger: Server-side template injection