# PLD DLH Instructions

## Add the target to your hosts file

Replace `<IP>` with the IP address provided in class, then run:

```bash
echo "<IP> academy.htb" | sudo tee -a /etc/hosts
```

---

## REQUIREMENTS

### TEAMWORK
- You will work in groups of 5.
- Everyone has to understand what is happening.
- The ones who understand must explain to those who don’t.
- This is a peer learning exercise: make sure no one is left behind and help each other.
- I’ll only help as a last resort if you are stuck for too long.

### NO CHEATING
- AI is prohibited.
- Google is your friend.
- This is not a prompt competition.
- You have to understand what is happening, not just copy and paste commands.

### ONLY USE MY WORDLISTS
- I’ll provide the wordlists needed for you to accomplish the task.

### NOTES
- Try forming a habit of taking notes.
- Write what worked, but also what didn’t, and why it didn’t.

### FINISHING EARLY
- For the groups who finish: make sure everybody understands.
- This is not a race.

---

## TASKS

1. Run a subdomain/vhost fuzzing scan on `academy.htb`. What subdomains can you identify?
2. Before running page fuzzing, first run an extension fuzzing scan. What different extensions are accepted by the domains?
3. One of the pages you identify should say `You don't have access!`. What is the full page URL?
4. In the page from the previous question, you should be able to find multiple parameters that are accepted by the page. What are they?
5. Fuzz the parameters you identified for working values. One of them should return a flag. What is the content of the flag?

---

## ANSWERS

> Fill in the answers below

1. Subdomains identified: 
2. Accepted extensions: 
3. Access denied page URL: 
4. Accepted parameters: 
5. Flag content:

---

## RESOURCES

### 1) Subdomain / virtual host fuzzing
A **subdomain** is a name that lives under a main domain, like `faculty.academy.htb`. A **virtual host** is a website served from the same IP address but identified using the `Host` header.  
This scan is used to discover hidden sites that point to the same server.

Important concepts:
- `-w` = the wordlist containing possible subdomains
- `FUZZ` = the placeholder `ffuf` replaces with each value from the wordlist
- `-u` = the target URL
- `-H` = adds an HTTP header, such as the `Host` header
- `-fs` = filters responses by size to ignore common false positives
- `subdomains-DLH.txt` = a list of common subdomains

### 2) Extension fuzzing
Many sites use hidden extensions such as `.php`, `.php7`, or `.phps`. Extension fuzzing helps identify which file types the server accepts before doing page fuzzing.

Important concepts:
- `indexFUZZ` = tests extensions by appending them to a filename such as `index.php`
- This scan helps you figure out which extensions are valid before scanning for pages
- `web-extensions-DLH.txt` = a list of common file extensions

### 3) Directory / page fuzzing
Directory fuzzing is used to discover hidden folders and pages. Recursion lets `ffuf` continue scanning inside newly found directories.

Important concepts:
- `-recursion` = keeps scanning discovered directories
- `-recursion-depth 1` = limits how deep recursion goes
- `-e` = adds extensions to each word from the wordlist
- `-fs` = filters out responses with the same size as normal error pages
- `directory_list-DLH.txt` = a list of common directory and file names

### 4) Parameter fuzzing
Parameter fuzzing is used to discover which input names a page accepts. This is useful when a page reacts differently depending on the parameter name.

Important concepts:
- `-X POST` = sends a POST request
- `-d` = sends form data in the request body
- `Content-Type: application/x-www-form-urlencoded` = tells the server the request body is normal HTML form data
- `parameters-DLH.txt` = a wordlist of common parameter names

### 5) Value fuzzing
This is a special case of parameter fuzzing where the parameter name is known, but the value is unknown. It helps identify accepted values.

Important concepts:
- `parameter=FUZZ` = replaces the username value with each word from the list
- `curl` = used for manual testing once you find a promising value
- `-X POST` = sends the request as POST
- `-d 'parameter=value'` = manually tests a specific value for a parameter
- `Content-Type: application/x-www-form-urlencoded` = keeps the request formatted as form data
- `usernames-DLH.txt` = a list of common usernames to test

### 6) What each scan is for
- **Subdomain/vhost fuzzing**: find hidden hostnames
- **Extension fuzzing**: discover which file extensions are accepted
- **Directory/page fuzzing**: find hidden paths and pages
- **Parameter fuzzing**: identify accepted parameter names
- **Value fuzzing**: find working values for a known parameter
