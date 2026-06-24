# Exercise 1 – Run HANA Cloud Backup Checks via SAP Automation Pilot

In this exercise, you will build your first command in SAP Automation Pilot - it will be about performing checks whether there is a recent backup on our SAP HANA Cloud instance. 


For a better understanding of the use case, refer to the diagram below:  

![](./images/ex-01-scenario-dsag.png)
---

### Exercise 1.1 – Build Your First Command in SAP Automation Pilot

Let’s create your first custom command.

1. Click  the **Commands** menu item from the left sidebar to navigate to the "Commands" section in SAP Automation Pilot 
   ![](./images/1-00-00-2-dsag.png)

2. Click **Create**.  
   ![](./images/ex01-03-dsag.png)

3. Fill in the details:  
   - **Catalog**: type in `DSAG HANA Ops Ex01 - Backup Checks` and from the drop-down select your DSAG HANA Ops Ex01 - Backup Checks catalog
   - **Name**: `simpleHanaCloudBackupCheck`  

   Click **Create**.  
   ![](./images/ex01-04-dsag.png)

4. The command is created but currently has no inputs, outputs, or executors.  
   

#### Add Inputs

1. Click **Add** under the **Input Keys** section.  
   ![](./images/1-2-2-05.png)

   Fill in:  
   - **Name**: `connectionUrl`  
   - **Type**: `string`
   - **Description**: JDBC connection URL for the database
   - Mark as **Required**  
   Click **Add**.  
   ![](./images/ex01-05-dsag.png)

 2. Add another input key to define the user which we'll use to call establish the connection to the DB:  
   - **Name**: `user`  
   - **Type**: `string`
   - **Description**: Name of a database user.
   - Mark as **Required**  
   Click **Add**.  
   ![](./images/ex01-06-dsag.png)

3. Add another input key to do a reference to an input with user's password:  
   - **Name**: `password`  
   - **Type**: `string`
   - **Description**: Password for the specified database user.
   - **Sensitive**: toggle the bar
   - Mark as **Required**  
   Click **Add**.  
   ![](./images/ex01-07-dsag.png)


4. Add another input key to define the DB backup threshold:  
   - **Name**: `ageThreshold`  
   - **Type**: `number`
   - **Description**: Backup age threshold in days
   - **Default Value Source**: `Static`  
   - **Value**: `3`  
   Click **Add**.  
   ![](./images/ex01-08-dsag.png)

You should get a **set of inputs** like the ones from the screenshot below - it is a crucial part of command contract as without inputs the command won't be possible to be executed successfully. 
![](./images/ex01-09-dsag.png)


#### Add Executor

1. Click **Add** under the **Executors** section.  
   ![](./images/ex01-10-dsag.png)

2. In the **Add Executor** dialog:  
   - Click **Here** to define the execution order  
   - **Alias**: `CheckBackup`  
   - **Command**: `ExecuteHanaCloudSqlStatement`  
   - Keep **Automap parameters** enabled  
   Click **Add**.  
   ![](./images/ex01-11-dsag.png)

   You will now see the executor added with mapped parameters.  
   ![](./images/ex01-12-dsag.png)

   Now you need to define the statment that will be executed to the DB. To do so, navigate to the Executors parameter section -> Click **Edit** and within the statement section add the following SQL query:

```sql
SELECT MAX(ENTRY_ID)
FROM SYS.M_BACKUP_CATALOG
WHERE ENTRY_TYPE_NAME = 'complete data backup' AND STATE_NAME = 'successful'
```
 ![](./images/ex01-17-dsag.png)
 ![](./images/ex01-18-dsag.png)

#### Add Outputs

1. Click **Add** under the **Output Keys** section.  
   ![](./images/ex01-13-dsag.png)

   Add the following outputs:  
   - **Name**: `backupResult` – **Type**: `string`  
   ![](./images/ex01-14-dsag.png)

3. Scroll down to **Configuration**, open the **output** executor, click **Edit**, and map the outputs to their respective values:  
   - **backupResult**: `$(.CheckBackup.output.result)`    
     
   Click **Update**.  
   ![](./images/ex01-15-dsag.png)
   ![](./images/ex01-16-dsag.png)


#### Trigger the Command

1. Click **Trigger**.  
   ![](./images/ex01-19-dsag.png)

2. Withhin the Inputs drop-down filed, pick up the iputs  **DsagOtherEnvHanaBindingCredentials** (there are the inputs for the URL connection, user and password needed for the command contract) and click **Trigger**.  
   ![](./images/ex01-20-dsag.png)

3. Once complete, click **Show** under **Output** to view the returned values.  
   ![](./images/ex01-21-dsag.png)

✅ The command executed successfully and returned outputs such as status, headers, and response body.
![](./images/ex01-22-dsag.png)

To convert it into a string, you need to transofrm the result. Go back to the executor "CheckBackup" --> Click **Edit** within the **Parameters** section -> **Advanced** Tab -> add the following expression with the filed **Result Transformer:**
`toArray[0][0][0]`

Click **Update** and retrigger the command. The output of the command shall look like this one which is the DB Backup ID: 
![](./images/ex01-23-dsag.png)

---

### [ 🧩 Note ] – Using Stored Input Keys
> **Note (Optional Exercise):**  
> This exercise is **optional**. Due to time limitations, it is **recommended to proceed with the next tasks** first.  
> You can return to the optional exercises **after completing the full scenario**, if time permits.

You can store and reuse input values in **SAP Automation Pilot** so that you are not asked to provide static parameters each time a command gets triggered but the input can be fetched dynamically or from already stored input key.

To explore the iputs  **DsagOtherEnvHanaBindingCredentials** from the leftsidebar -->  **Inputs** -->  **DsagOtherEnvHanaBindingCredentials** and you will see all stored input keys we already have used. 
![](./images/ex01-24-dsag.png)
![](./images/ex01-25-dsag.png)

_Note: sensitve data / inputs is save to be stored as data gets encyrpeted when marked as sensitve._

✅ You’ve learned how to create, execute, and reuse inputs for commands in SAP Automation Pilot.

---

## Exercise 1.2 – Exploring the other commands in the catalog "DSAG HANA Ops Ex01 - Backup Checks" 

Now, let’s go back to the catalog **DSAG HANA Ops Ex01 - Backup Checks** --> **Commands** --> **02GetHanaCloudBackup**
![](./images/ex01-26-dsag.png)
![](./images/ex01-27-dsag.png)

### Check executor: "checkLatestBackupStart"
**command**: `sql-sapcp:ExecuteHanaCloudSqlStatement`
**statement**:
```sql
SELECT SYS_START_TIME
FROM SYS.M_BACKUP_CATALOG
WHERE ENTRY_ID = $(.CheckBackup.output.result);
```
- Result Transformer (within the Advanced tab section): `toArray[0][0][0]`
![](./images/ex01-28-dsag.png)

**Validation**:
- Actual Value: `$(nowMillis - (.CheckBackup.output.result | toNumber) < (.execution.input.ageThreshold * 24 * 60 * 60 * 1000))`
- Operator: `equals`
- Expected value: true
### Understanding the Validation Logic

The validation compares the timestamp of the latest successful backup with the threshold specified by the user.

This allows SAP Automation Pilot to determine whether the database has been backed up recently enough to meet operational requirements.

For example:

- Last backup 1 day ago → Success
- Last backup 2 days ago → Success
- Last backup 5 days ago with threshold set to 3 days → Failure

Such validations help operations teams identify backup issues before they become business-critical incidents.

**Error Message**: 
- Message: `No database backup in $(.execution.input.ageThreshold) day(s). Last backup was on $(.CheckBackup.output.result | toNumber | toDate("yyyy-MM-dd HH:mm:ss"))`
- Actual Value: `$(.CheckBackup.output.errorCode == null)`
- Operator: `equals`
- Expected value: true
![](./images/ex01-29-dsag.png)

#### Why Is This Error Message Important?

Instead of returning raw SQL output, SAP Automation Pilot converts technical information into meaningful operational insights.
This allows operators, monitoring tools, and AI Agents to understand immediately whether action is required without interpreting database-specific details.

### New output `backupStartTime`
A new outoput key that returns when the backup had been started: 
name: `backupStartTime`
type: `string`

Mapping the output within the output section: 
- values for the ouput: `$(.checkLatestBackupStart.output.result)`

### Triger the command
Now trigger the command and you will get also details about when the last DB back up was initiated (see below). 
![](./images/ex01-30-dsag.png)

---

## Summary

You have successfully:  
- Explored SAP Automation Pilot
- Created and executed commands
- Understand better command contract concept  

Proceed to the next step:  
➡️ [Exercise 2 – Run HANA Cloud Audit logs check ](../ex2/README.md)
