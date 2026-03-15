<h2>Group Policy Account Lockout Configuration</h2>

<p>
<img width="800" alt="Group Policy Object Overview"
src="images/GroupPolicy.jpeg" />
</p>

<p>
Group Policy Objects (GPOs) are used in Active Directory environments to centrally manage and enforce configuration settings across computers and users in a domain. 
Administrators use Group Policy to apply security rules, password policies, software configurations, and many other system settings from the domain controller.
</p>

<p>
In this lab, Group Policy is used to configure an <b>Account Lockout Policy</b>. This policy protects user accounts from brute-force attacks by locking the account after a certain number of failed login attempts. When the threshold is reached, the account becomes locked and the user cannot log in again until the lockout timer expires or an administrator manually unlocks the account.
</p>

<br><br>


<h3>Opening the Group Policy Management Console</h3>

<p>
<img width="700" alt="Run gpmc.msc"
src="images/01-AD-Group-Policy.png" />
</p>

<p>
The first step is opening the <b>Group Policy Management Console (GPMC)</b>. 
This console is where administrators manage domain policies and control how security rules apply to users and computers.
</p>

<p>
To open it, press <b>Windows + R</b> to open the Run dialog box and type:
</p>

<pre>gpmc.msc</pre>

<p>
This command launches the Group Policy Management Console. From here, administrators can edit existing domain policies or create new Group Policy Objects that will apply across the domain.
</p>

<p>
Since Group Policy affects the entire domain environment, administrative privileges are required to access and modify these settings.
</p>

<br><br>


<h3>Navigating to the Account Lockout Policy</h3>

<p>
<img width="900" alt="Account Lockout Policy Location"
src="images/02-AD-Group-Policy.png" />
</p>

<p>
After opening the Group Policy Management Editor, the administrator navigates through the following path in the policy tree:
</p>

<pre>
Computer Configuration
   → Policies
      → Windows Settings
         → Security Settings
            → Account Policies
               → Account Lockout Policy
</pre>

<p>
This section of Group Policy contains security rules that control how the system responds when multiple failed login attempts occur.
</p>

<p>
Initially, these settings may show as <b>Not Defined</b>, meaning the domain has not yet enforced a lockout rule. Without this protection, attackers could attempt unlimited password guesses against user accounts.
</p>

<p>
Configuring these policies adds an important security layer that helps defend against brute-force authentication attacks.
</p>

<br><br>


<h3>Configuring the Account Lockout Settings</h3>

<p>
<img width="900" alt="Configured Lockout Policy"
src="images/03-AD-Group-Policy.png" />
</p>

<p>
The account lockout policy is now configured with specific security values.
</p>

<p>
The settings applied are:
</p>

<ul>
<li><b>Account lockout duration:</b> 30 minutes</li>
<li><b>Account lockout threshold:</b> 5 invalid login attempts</li>
<li><b>Allow administrator account lockout:</b> Enabled</li>
<li><b>Reset account lockout counter after:</b> 10 minutes</li>
</ul>

<p>
This means that if a user enters the wrong password <b>five times</b>, the system will automatically lock the account for <b>30 minutes</b>.
</p>

<p>
The reset counter setting means that if fewer than five failed attempts occur and the user waits <b>10 minutes</b>, the system will reset the attempt counter back to zero.
</p>

<p>
These settings are commonly used in enterprise environments to prevent automated password guessing attacks.
</p>

<br><br>


<h3>Verifying the Domain Policy Settings</h3>

<p>
<img width="900" alt="Default Domain Policy Settings"
src="images/04-AD-Group-Policy.png" />
</p>

<p>
The Group Policy Management console displays the current security configuration for the domain’s <b>Default Domain Policy</b>.
</p>

<p>
This view confirms that the account lockout policy has been successfully applied to the domain.
</p>

<p>
Administrators can review all security policies here, including:
</p>

<ul>
<li>Password complexity requirements</li>
<li>Password history enforcement</li>
<li>Password age policies</li>
<li>Account lockout settings</li>
</ul>

<p>
Since the Default Domain Policy applies to all domain users, these security rules will automatically apply to every account within the Active Directory domain.
</p>

<br><br>


<h3>Testing the Lockout Policy with Remote Desktop</h3>

<p>
<img width="900" alt="Account Locked Error"
src="images/05-AD-Group-Policy.png" />
</p>

<p>
To verify that the policy works correctly, the user attempts to log into a domain machine multiple times using incorrect credentials through Remote Desktop.
</p>

<p>
After exceeding the configured threshold of <b>five failed login attempts</b>, the system locks the user account.
</p>

<p>
The Remote Desktop client displays the following message:
</p>

<p>
<b>"Unable to connect. The user account has been locked due to too many sign-in attempts."</b>
</p>

<p>
This confirms that the Group Policy lockout configuration is functioning properly. 
The account will remain locked until either the lockout duration expires or an administrator manually unlocks the account.
</p>

<br><br>


<h3>Unlocking the Locked User Account</h3>

<p>
<img width="900" alt="Unlock Account in Active Directory"
src="images/06-AD-Group-Policy.png" />
</p>

<p>
To restore access, an administrator opens <b>Active Directory Users and Computers</b> and locates the affected user account.
</p>

<p>
Inside the user’s properties window, the system shows a message indicating that the account has been locked due to the lockout policy.
</p>

<p>
Administrators can resolve this by selecting the option:
</p>

<p>
<b>"Unlock account. This account is currently locked out."</b>
</p>

<p>
Once this option is checked and applied, the user account is unlocked and can log in again immediately without waiting for the lockout timer to expire.
</p>

<br><br>


<h3>Confirming the Logged-In User</h3>

<p>
<img width="900" alt="whoami command"
src="images/14-AD-Group-Policy.png" />
</p>

<p>
After the account is unlocked and the user successfully logs back into the system, the <b>whoami</b> command is used in the command prompt to verify the currently authenticated user.
</p>

<pre>whoami</pre>

<p>
The output shows:
</p>

<pre>mydomain\bab.gur</pre>

<p>
This confirms that the user is successfully authenticated within the Active Directory domain and that access has been restored after unlocking the account.
</p>

<p>
Using commands like <b>whoami</b> is a quick and reliable way to confirm the identity of the currently logged-in user, especially when testing domain authentication and Group Policy behavior.
</p>

<h3>Disabling the User Account</h3>

<p>
<img width="900" alt="Disable User Account"
src="images/07-AD-Group-Policy.png" />
</p>

<p>
In this step, the administrator opens <b>Active Directory Users and Computers</b> and navigates to the organizational unit that contains the user accounts. 
Inside the <b>_EMPLOYEES</b> Organizational Unit, the user account <b>bab.gur</b> is located.
</p>

<p>
To simulate another security scenario, the administrator right-clicks the user account and selects <b>Disable Account</b>.
</p>

<p>
Disabling an account prevents the user from authenticating to any system in the domain. 
This is commonly done when an employee leaves the company, when an account is suspected to be compromised, or when administrators need to temporarily block access without deleting the account entirely.
</p>

<p>
Unlike deleting an account, disabling it preserves the user's information, group memberships, and permissions, allowing administrators to easily re-enable the account later if needed.
</p>

<br><br>


<h3>Confirmation that the Account Was Disabled</h3>

<p>
<img width="900" alt="Account Disabled Confirmation"
src="images/08-AD-Group-Policy.png" />
</p>

<p>
After selecting <b>Disable Account</b>, Active Directory confirms the action with a notification message stating:
</p>

<p>
<b>"Object bab.gur has been disabled."</b>
</p>

<p>
This confirms that the user account is no longer allowed to authenticate within the domain. 
At this point, any attempt to log in using this account will be rejected by the domain controller.
</p>

<p>
Administrators can visually identify disabled accounts because the user icon in Active Directory appears faded or marked differently compared to active accounts.
</p>

<br><br>


<h3>Resetting the User Password</h3>

<p>
<img width="900" alt="Reset Password Window"
src="images/09-AD-Group-Policy.png" />
</p>

<p>
Administrators also have the ability to reset user passwords directly from Active Directory.
</p>

<p>
In this step, the administrator selects the user account and chooses <b>Reset Password</b>. 
A dialog window appears allowing the administrator to enter a new password and confirm it.
</p>

<p>
This feature is commonly used by IT support teams when users forget their passwords or when a security incident requires immediate credential changes.
</p>

<p>
The window also provides options such as:
</p>

<ul>
<li>Forcing the user to change their password at next login</li>
<li>Unlocking the user account if it was locked due to failed login attempts</li>
</ul>

<p>
Resetting passwords through Active Directory ensures the change is immediately applied across the domain.
</p>

<br><br>


<h3>Attempting to Log in With a Disabled Account</h3>

<p>
<img width="900" alt="RDP Disabled Account Error"
src="images/10-AD-Group-Policy.png" />
</p>

<p>
After disabling the account, the user attempts to connect to the system again using <b>Remote Desktop</b>.
</p>

<p>
Because the account has been disabled by the administrator, the authentication request is rejected by the domain controller.
</p>

<p>
The Remote Desktop client displays an error message stating that the account has been disabled and instructs the user to contact the network administrator.
</p>

<p>
This behavior confirms that Active Directory is successfully enforcing account status rules and preventing disabled accounts from accessing domain resources.
</p>

<br><br>


<h3>Opening Event Viewer to Investigate Security Logs</h3>

<p>
<img width="700" alt="Open Event Viewer"
src="images/11-AD-Group-Policy.png" />
</p>

<p>
To investigate authentication activity and security events, administrators use the Windows <b>Event Viewer</b>.
</p>

<p>
The Event Viewer can be opened using the Run dialog by typing:
</p>

<pre>eventvwr.msc</pre>

<p>
Event Viewer allows administrators to review system logs that record important security events such as successful logins, failed login attempts, account lockouts, and policy changes.
</p>

<p>
These logs are critical for troubleshooting issues and for security monitoring within enterprise environments.
</p>

<br><br>


<h3>Event Viewer Overview</h3>

<p>
<img width="900" alt="Event Viewer Overview"
src="images/12-AD-Group-Policy.png" />
</p>

<p>
Once Event Viewer opens, administrators can navigate through several log categories including:
</p>

<ul>
<li>Application Logs</li>
<li>System Logs</li>
<li>Security Logs</li>
<li>Forwarded Events</li>
</ul>

<p>
The most important log for authentication and security monitoring is the <b>Security</b> log located under <b>Windows Logs</b>.
</p>

<p>
This log records detailed information about user authentication events and security-related activities occurring on the system.
</p>

<br><br>


<h3>Searching Security Logs for the User Activity</h3>

<p>
<img width="900" alt="Search Security Logs"
src="images/13-AD-Group-Policy.png" />
</p>

<p>
Inside the Security log, the administrator can search for specific events related to a particular user.
</p>

<p>
Using the <b>Find</b> feature, the administrator searches for the username:
</p>

<pre>bab.gur</pre>

<p>
This allows the administrator to quickly locate events associated with that user account.
</p>

<p>
For example, the displayed event shows a logoff event with <b>Event ID 4634</b>, which indicates that a user session has ended.
</p>

<p>
Security logs provide administrators with detailed auditing information that can help track user activity and identify potential security incidents.
</p>

<br><br>


<h3>Viewing Failed Login Attempts</h3>

<p>
<img width="900" alt="Audit Failure Log"
src="images/15-AD-Group-Policy.png" />
</p>

<p>
Another important type of event recorded in the Security log is an <b>Audit Failure</b>.
</p>

<p>
In this example, the log shows <b>Event ID 4625</b>, which indicates that an account failed to log in.
</p>

<p>
This event includes detailed information such as:
</p>

<ul>
<li>The time of the failed login attempt</li>
<li>The system where the login was attempted</li>
<li>The account that attempted to authenticate</li>
<li>The reason the authentication failed</li>
</ul>

<p>
Security administrators often monitor these logs to detect suspicious activity, such as repeated login failures that may indicate a brute-force attack or unauthorized access attempts.
</p>

<p>
By combining Group Policy security settings with monitoring tools like Event Viewer, organizations can better protect their Active Directory environments and quickly investigate authentication-related issues.
</p>
