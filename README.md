To provide answers for the questions, I'll outline the solutions based on the image provided:

### SET A

#### 1. MapReduce Program
The task is to write a MapReduce program to process the text as shown in the diagram.

**MapReduce Word Count Program:**
```python

def mapper(text):
    words = text.split()
    for word in words:
        yield f"{word}\t1"


def reducer(key, values):
    total = sum(values)
    print(f"{key}\t{total}")


input_text = ["Deer Bear River", "Car Car River", "Deer Car Bear"]


intermediate = []
for line in input_text:
    intermediate.extend(list(mapper(line)))


from collections import defaultdict

grouped = defaultdict(list)
for entry in intermediate:
    word, count = entry.split("\t")
    grouped[word].append(int(count))


for key in grouped:
    reducer(key, grouped[key])
```


**Steps:**
 **Configure RSA 2-Factor Authentication:**
1. Generate the ssh key pair using `ssh-keygen` command on the local system.
 

3.	Upload the public key to the CentOS server.
```
cat ~/.ssh/id_rsa.pub | ssh username@remote_host "mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys"
```
4.	Change the file permissions using the commands ‘chmod 700 ~/.ssh/’ and ‘chmod 600 ~/.ssh/authorized_keys’ and then reload the service.
 ```
chmod 700 ~/.ssh/
chmod 600 ~/.ssh/authorized_keys
systemctl reload sshd
```

5.	Login to the CentOS server from the local computer.
 ```
ssh username@ip
```


**Install Apache Server:**
   - Install Apache:
     ```bash
     sudo yum install httpd firewalld
     sudo systemctl start httpd
     sudo systemctl enable httpd
     sudo firewall-cmd --permanent --add-service=http
     sudo firewall-cmd --reload
     ifconfig
     ```
     After ifconfig, paste the ipaddress in the browser.

### SET B

#### 3. MapReduce Program
Similar to Set A, we need to write a MapReduce program for the given input and diagram.

**MapReduce Word Count Program:**
```python
def mapper(text):
    words = text.split()
    for word in words:
        yield f"{word}\t1"

def reducer(key, values):
    total = sum(values)
    print(f"{key}\t{total}")

input_text = ["Bus Car Train", "Train Plane Car", "Bus Bus Plane"]

intermediate = []
for line in input_text:
    intermediate.extend(list(mapper(line)))

from collections import defaultdict

grouped = defaultdict(list)
for entry in intermediate:
    word, count = entry.split("\t")
    grouped[word].append(int(count))

for key in grouped:
    reducer(key, grouped[key])
```


**Configure RSA 2-Factor Authentication:**
   - Install RSA Authentication Agent:
     ```bash
     sudo yum install rsa-agent
     ```
   - Configure the agent according to the RSA setup guide.
**Install MariaDB Server:**
   - Install MariaDB:
     ```bash
     sudo yum install mariadb-server
     sudo systemctl start mariadb
     sudo systemctl enable mariadb
     ```

