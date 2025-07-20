1. Open the Start menu, type **gpedit.msc**, and press Enter.

2. In the Group Policy Editor, navigate to: **Local Computer Policy** → **Computer Configuration** → **Windows Settings** → **Security Settings** → **Local Policies** → **Audit Policy**.

3. Double-click on **Audit Logon Events**. In the window that appears, check both the "Success" and "Failure" options, then click OK.

4. Return to the Start menu, type **Event Viewer**, and press Enter.

5. In the Event Viewer, expand **Windows Logs** and select **Security**.

6. Look for events with Event ID **4624**, which indicate successful login attempts.

7. Double-click on any event to view the time and additional details about the login.

