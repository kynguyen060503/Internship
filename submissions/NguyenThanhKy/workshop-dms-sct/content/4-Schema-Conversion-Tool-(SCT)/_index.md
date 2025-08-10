---
title: "Schema Conversion Tool (SCT)"
date: 2025-08-09T00:00:00+07:00
weight: 4
chapter: false
pre: " <b> 4. </b> "
---

**Content:**
- [Download & Install SCT](#download--install-sct)
- [Create SCT Project](#create-sct-project)
- [Schema Analysis & Conversion](#schema-analysis--conversion)

---

### Download & Install SCT

#### Download SCT
1. Go to: [AWS SCT](https://aws.amazon.com/dms/schema-conversion-tool/).  
2. Download the version suitable for your OS.  
3. Install following the instructions.

#### Setup JDBC Drivers
1. Download **MySQL Connector/J**.  
2. Download **PostgreSQL JDBC Driver**.  
3. Copy both `.jar` files into the **SCT drivers** directory.

---

### Create SCT Project

#### Create a New Project
1. Open **AWS SCT**.  
2. Go to **File > New Project**.  
3. Project name: `MySQL-to-PostgreSQL-Migration`  
4. Location: Select a folder to save the project.  
5. **Source database:** MySQL  
6. **Target database:** PostgreSQL

![Monitor Task Status](/images/4-/sct1.png)

#### Connect to Source Database (MySQL)
1. Right-click **MySQL** in the **source panel**.  
2. Select **Connect to MySQL**.  
3. Configuration:  
Server name: [SOURCE-MYSQL-ENDPOINT]
Server port: 3306
User name: admin
Password: MyPassword123!
Database: sampledb


> Using localhost if you run this cmd line ***ssh -i migration-key.pem -L 3306: [SOURCE-MYSQL-ENDPOINT]:3306 ec2-user@[EC2 IPv4]***)

![Monitor Task Status](/images/4-/sct2.png)


4. Click **Test Connection** → **OK** if successful.

![Monitor Task Status](/images/4-/sct3.png)

#### Choose a schema

![Monitor Task Status](/images/4-/sct4.png)

#### Connect to Target Database (PostgreSQL)
1. Right-click **PostgreSQL** in the **target panel**.  
2. Select **Connect to PostgreSQL**.  
3. Configuration:  
Server name: [TARGET-POSTGRES-ENDPOINT]
Server port: 5432
User name: postgres
Password: MyPassword123!
Database: targetdb
> Using localhost if you run this cmd line ***ssh -i migration-key.pem -L 5432:[TARGET-POSTGRES-ENDPOINT]:5432 ec2-user@[EC2 IPv4]***)

![Monitor Task Status](/images/4-/sct5.png)

4. Click **Test Connection** → **OK**.

![Monitor Task Status](/images/4-/sct6.png)
---

### Schema Analysis & Conversion

#### Analyze Source Schema
1. Expand **sampledb** in the **source panel**.  
2. Right-click **sampledb** → **Create Report**.  
3. View the results in the **Summary** tab.

![Monitor Task Status](/images/4-/sct7.png)

![Monitor Task Status](/images/4-/sct8.png)

#### Convert Schema
1. Right-click **sampledb** → **Convert Schema**.  
2. Review the converted schema in the **target panel**.  
3. Check for **warnings/errors**.

#### Apply to Target
1. Right-click the converted schema in the **target panel**.  
2. Select **Apply to database**.  
3. Review the SQL statements.  
4. Click **Apply to database**.

![Monitor Task Status](/images/4-/sct9.png)