# Assess and Assure Security Posture Across Your Fleet of Databases

## Introduction

Learn how to secure and ensure compliance of your enterprise databases in this workshop. Discover methods to monitor their security, validate configurations, and automate compliance with company, industry, and regulatory standards like CIS and STIG. Explore using Oracle Enterprise Manager to automate inventory and baseline all database targets, including various versions and instances deployed over time.

*Estimated Time*: 60 minutes

### Secure database assets using Fleet Maintenance Hub in Oracle Enterprise Manager

Starting with Enterprise Manager 13.5 RU16, the new Fleet Maintenance Hub simplifies automated database updates and upgrades. This hub identifies security risks, provides patch recommendations, guided workflow for image creation and enables efficient scheduling and monitoring of patching and upgrade operations. It manages all database types, ensures compliance with patch policies, and centralizes database maintenance for enhanced security and stability.

### Compliance check of your database assets using Oracle Enterprise Manager
<<Data from Shiva>>


### Objectives

In this lab you will perform the following steps:
| Step No. | Feature                                                    | Approx. Time | Details                                                                                                                                                                    | Value Proposition |
|----------------------|------------------------------------------------------------|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|
| 1                    | Assess patch recommendation and create gold image                             | 15 minutes  | Review the patch recommendations for existing gold images                                                                                                                 | Experience the benefits of using these recommendations, which automate traditionally manual and time-consuming tasks to produce highly accurate results.                  |
| 2                    | Secure databases by updating with new gold image | 25 minutes  | Update databases using Gold image | Effortlessly update your pluggable database, saving time and ensuring seamless operations.                   |
| 3                    | Elevate security posture by auditing for compliance | 25  minutes  | Refresh a Gold Image based on latest patch recommendation | Streamline the process of creating a new version for a gold image with these steps. The latest version ensures an efficient update and upgrade process.


### Prerequisites
- A Free Tier, Paid or LiveLabs Oracle Cloud account
- You have completed:
    - Lab: Prepare Setup (*Free-tier* and *Paid Tenants* only)
    - Lab: Environment Setup
    - Lab: Initialize Environment

*Note*: This lab environment is setup with Enterprise Manager Cloud Control Release 13.5 and Database 19.10 as Oracle Management Repository. Workshop activities included in this lab will be executed both locally on the instance using Enterprise Manager Command Line Interface (EMCLI) or Rest APIs, and the Enterprise Manager console (browser)

## Task 1: Assess patch recommendation and create gold image

In this task, you will review patch recommendations for existing gold images and create a new version based on those recommendations.


1. Login to Enterprise Manager as user - emadmin. Copy and paste or type in the following username and password credentials into the fields.


    ```
    Username: <copy>emadmin</copy>
    ```

    ```
    Password: <copy>welcome1</copy>
    ```

   ![](images/em-login.png "em-login")

2. Once logged in, navigate to ***Targets >> Databases***  

    ![](images/em-navig1.png "em-navig1")

    and then ***Administration >> Fleet Maintenance Hub***

   ![](images/em-navig2.png "em-navig2")

3. Fleet Maintenance Hub is one stop place for end to end patch assessment and performing patching operation.

    ![](images/em-hub.png "em-hub")

    In the top right corner, you see the status of ***Last Patch Recommendation Update***. If a date is shown below it, then the patch recommendation was executed on that date.

<!--
To setup patch recommendation, review [Oracle Enterprise Manager](https://docs.oracle.com/en/enterprise-manager/cloud-control/enterprise-manager-cloud-control/13.5/emlcm/downloading-patch-recommendations-and-patches.html).
-->


<!--
   To successfully complete this lab, do not upload any new patch catalogs or enter your MOS credentials as described in the above documentation. Doing so will generate new recommendations that may differ from those in the upcoming sections.
-->


4. Click on Tile 2 of the Fleet Maintenance Hub, referred to as Patch Recommendations for Images.

    In this tile, you can review the health of your gold images. If a gold image is shown green ( listed under up to date ), then that particular gold image is healthy and contains all recommended patches. This image should be used for patching or upgrade operation.

    Similarly, if a gold image is shown red ( listed under Images with patch recommendations ), then that suggests that the particular gold image lacks critical recommended patches and should not be used for patching or upgrade operation.

    Administrator should create a new version for such images.

    ![](images/tile2.png "hub-tile2")

    Here, we see that goldimages - ***19cDB-Linux-x64-ERP*** has two patch recommendations, while ***19cDB-Linux-x64-APPS*** has a green checkmark.

    This indicates that 19cDB-Linux-x64-ERP should have a new version that includes the two recommended patches, whereas 19cDB-Linux-x64-APPS is up to date and can be used for patching.

    For this lab, we will use the ***19cDB-Linux-x64-APPS*** image to perform pdb patching.

<!--
Follow the [Link](https://docs.oracle.com/en/enterprise-manager/cloud-control/enterprise-manager-cloud-control/13.5/emlcm/image-maintenance-ui.html) to understand the steps involved in refreshing a gold image.
-->

5. In the next few steps, we will use the Fleet Maintenance Hub to refresh gold image ***19cDB-Linux-x64-ERP***.

    While the refresh process is running, to complete the lab within the given timeline, we will use ***19cDB-Linux-x64-APPS*** to secure one of the databases.

6. Let's refresh the gold image - ***19cDB-Linux-x64-ERP***

    As observed in step 4, ***19cDB-Linux-x64-ERP*** has two patch recommendations.

    ![](images/patch-recommendation.png "patch-recommendation")

    Click on the numeric value 2.

    A new slideout will appear.

    ![](images/patch-slideout.png "slideout")

7. The next step is to create a new version in 19cDB-Linux-x64-ERP that will include the recommended patches.

    Close the slideout.

    ![](images/create-version.png "create version")

    Click on the ***"Create New Version"*** link under the Patch Recommendation column.

    Alternatively, click on the menu under Actions and create a new version.

8. In the new window, we need to provide source details, which act as input for version creation and version details.

    In the left-hand section, we first select the Oracle Home, which will be used as the source Oracle Home.

    ![](images/version-options.png "options")

    - Select Oracle Home.
      1. Click on the ***Select Oracle Home*** button. In the new screen select the Oracle Home ending with ***3383***.
      2. Select Include Patches.
      3. Select one of the radio buttons. You can either choose to clone the above selected Oracle Home, apply recommended patches to the cloned Oracle Home, and then use this home to create a new version. Alternatively, you can select the option to patch the Oracle Home and use it to create a new version.
      We will select the latter option - ***Patch the selected Oracle Home***, as shown in the image.

    - Under Work Directory, provide location where logs will be created. You may enter location as ***/tmp/u01***
    - Credentials - Provide Normal and Privileged Credentials to complete this operation.

        For Normal Host credentials, from the drop down select ***ORACLE***.
        For Privileged Host credentials, from the drop down select ***ROOT***.

    In the right hand section, provide
    - New version name that we are creating. You may enter ***v19.23DBRU***
    - Select Storage options. This will determine the source and location where the new version will be saved. We will select the default values as shown in the image.


    Click ***Next***.

9. In this screen, select the two patches that we have already downloaded and uploaded in the software library.

    ![](images/rec-patches.png "rec-patches")

    Click ***Next***.

10. In the next page, review the selections that we made on the previous screen. Then, click on ***Submit*** to initiate the operation to create a new version.

    ![](images/submit-version.png "submit-version")

11. Once you submit, a new Deployment Procedure (DP) is initiated. Click on the DP name to review the steps.

    ![](images/DP.png "DP")

    lets stay on the DP page for 2 minutes.

    Note: The submitted Deployment Procedure above may fail. If you encounter the error below, click on "Ignore" and select "OK" in the confirmation window.
    ![](images/error-DP.png "error-DP")

    However, to complete this lab, we will proceed to the next step.

## Task 2: Secure databases by updating with new gold image

In this task, we will perform Pluggable Database patching.

1. Lets navigate to Databases Homepage, by clicking on ***Targets >> Databases***.

   ![](images/em-navig11.png "em-navig1")

    In this scenario, we will unplug the ***Finance PDB*** (associated with the Container database - ***sales.subnet.vcn.oraclevcn.com***) and plug it into a higher version CDB, ***hr.subnet.vcn.oraclevcn.com***.
    Currently, Finance is at version 19.17, while HR is at version 19.23.

    ![](images/pre-update.png "pre-update")

    Lets complete below steps to perform the pdb patching.

2. Subscribe sales CDB to goldimage ***19cDB-Linux-x64-APPS***.

    Navigate to Fleet Maintenance Hub, by clicking on ***Administration >> Fleet Maintenance Hub***

    ![](images/em-navig2.png "em-navig2")

    In the Fleet Maintenance Hub, under the "Target Subscription" tab in Tile 1, follow these steps:

    - Click on the ***Subscribe*** button.
    - Select filter ***19*** under Release.
    - From the dropdown, select the goldimage - ***19cDB-Linux-x64-APPS***.
    - From the list of databases, select ***sales.subnet.vcn.oraclevcn.com***.
    - Click on ***Subscribe*** at the top right corner.

    ![](images/sales-subscribe.png "subscribe")

    Upon completion, click on ***Close***.

3. Navigate to Tile 3 - ***Target Patch Compliance*** in the Fleet Maintenance Hub.

    ![](images/tile3.png "tile3")

    In tile 3, we see that the sales CDB is associated with goldimage ***19cDB-Linux-x64-APPS***.
    Under Target Compliance columns, we see that the CDB is drifted and needs to be patched. In this lab, we will perform a PDB level patching.

    Click on the menu under Actions for the sales CDB, and select ***Update Pluggable Database***. This will launch the operator UI of Fleet Maintenance.

4. We are now at the operator UI screen, with pre-selected values for Gold Image, Target Type and Operation.

    ![](images/patching-ui1.png "patching-ui1")

    Select Finance pdb and click ***Next***.

5. In this page, we will select relevant options and enter values wherever required.

    - Under Maintenance Task, select ***Attach Existing CDB***.
    - Under Attach Existing CDB, review the source CDB, which is sales.
    - Under Destination CDB, select HR.
    - Under credentials, select values from the drop-down menu as per the image.
        - Normal Host Credentials as ***ORACLE***
        - Privileged Host Credentials as ***ROOT***
        - SYSDBA Database Credentials as ***SYS_SALES***
    - Under Work Directory, enter ***/u01/tmp***
    - Under Options section, select ***Yes*** for Skip Cluster Verification Utility Checks.

    ![](images/patching-ui2.png "patching-ui2")

    Click ***Next***.

6. Click on ***Validate*** and then select ***Quick Validation***.

    ![](images/patching-ui3.png "patching-ui3")

    Once you receive the successful validation message, click ***Close*** and then hit ***Submit***.

7. A new dialogue box will ask for the name of the deployment procedure.
    This unique name will allow you to track the operation. We have provided the name "Demo_update".

    ![](images/patching-ui4.png "patching-ui4")

    Copy and paste or type in the following into the Submission Procedure Name prefix.

        ```
        <copy>Demo_update</copy>
        ```

8. There are two Deployment Procedure submitted.
    - Attach Existing CDB
    - Update PDB

    Click on ***Monitor Progress***, which will open a new window.

    ![](images/patching-ui5.png "patching-ui5")

9. In the new page, under search, enter ***Demo*** so that you only see the two Deployment procedures associated with this lab.

    ![](images/patching-ui6.png "patching-ui6")

    We see that the Deployment procedure with the name "Attach" has completed successfully.

    Let's click on the Deployment procedure with the name "Update" and review the steps performed.

    ![](images/patching-ui7.png "patching-ui7")

9. With both Deployment procedures completed successfully, let's go back to the databases homepage by navigating to ***Targets -> Databases***.
    ![](images/patching-ui8.png "patching-ui8")

    We see that the Finance PDB has moved out of the sales CDB and is now plugged into the HR CDB, running at version 19.23

Now that you have patched PDB with recommended patches, it is also important to elevate the security posture by putting in relevant security control and audit for compliance. The next step will help you gain understanding on how to complete the relevant steps.

## Task 3: Elevate security posture by auditing for compliance or Compliance Management for Pluggable Database

Now that you have applied security patches to your pluggable database, you want to ensure the configuration, access and user privileges are secure and audit for compliance with CIS Benchmarks which is the industry standard.

Enterprise Manager provides solution to secure and ensure compliance with security policies defined by your security office and auditors. With this solution, you can secure the entire stack from databases, underlying Linux hosts and Exadata infrastructure with out-of-box security controls.  

![compliance stack](images/stack-security-compliance.png " ")

For Oracle Databases 12c and 19c, CIS Benchmark is an industry compliance standards that is available out-of-the-box. Besides that, Department of Defense (DoD) Security Technical Implementation Guide (STIG) standard is available out-of-the-box for regulatory requirements.

For underlying Linux hosts, built-in SCAP engine enables security standards like STIG, PCI-DSS, HIPAA are available.

Finally, for Exadata family of Engineered Systems, compliance standards tied to Autonomous Health Framework (AHF) Exachk is available out-of-box.

In this lab, you will get hands-on experience with ***Oracle 19c Database CIS V1.1.0 - Level 1 - RDBMS using Unified Auditing for Oracle Pluggable Database*** to secure configuration of provisioned database.

Center for Internet Security (CIS) benchmark for Oracle database provides comprehensive set of security controls, and configuration guidelines to secure against cyber threats and data breaches. It enables in keeping your databases security posture at elevated levels and check for compliance with your auditors.

Lets start the lab.

To begin, lets take a look at available CIS Benchmarks and choose one of them for associating a database target for security assessment.

1. From the Enterprise menu, select **Compliance, then select Library**.

    ![Navigate to Library](images/enterprise-compliance-library.png " ")

2. Click the **Compliance Standards** tab.

    You will list of out-of-box Compliance Standards. Lets' look for CIS Benchmarks.

    Search for "Oracle 19c Database CIS".

    In the **Applicable To** dropdown, select **Pluggable Database**.

    Click **Search**.

    ![Search CIS](images/search-cis-pluggable-std.png "  ")

    Select the row **Oracle 19c Database CIS V1.1.0 - Level 1 - RDBMS using Unified Auditing for Oracle Pluggable Database**, then click **Associate Targets** tab.

    ![Target associate](images/cis-compliance-associate-target.png " ")

3. Click **Add** and Select **hr.subnet.vcn.oraclevcn.com_FINANCE** PDB you wish to associate.

    In the **Target name**, type **_FINANCE** and click **Search**.

    ![Add PDB](images/add-cis-search-pdb.png " ")

    Choose  **hr.subnet.vcn.oraclevcn.com_FINANCE**.

    Click Select.

    ![Add PDB](images/select-cis-pdb-target.png " ")

4. Verify the PDB selected and click **OK**.

    ![Enable CIS](images/enable-status-pdb.png " ")

    In the Save Association dialog box, click **Yes**.

    ![Confirm Association ](images/save-association-pdb.png "")

5. Click **OK** on the Information pop-up window.

    ![Confirm Association ](images/compliance-std-target-process.png " ")

6. Compliance Results Analysis

    You have completed one-time setup process.

    Lets' analyze the results.

    Now, Navigate to ***Enterprise >> Compliance >> Dashboard***

    ![Navigate to result](images/navigated-dashboard.png " ")

    At this point, all CIS security controls is getting processed, will take approximately few minutes to complete.

    Refreshing the icon ![Refresh](images/refresh-icon.png "  ")  ***in Compliance Dashboard page***.

    ![Navigate to result](images/navigated-to-dashboard.png  " ")

    In this dashboard, you can explore each dashlet:

    **Targets Evaluated:** Summarizes the number of databases, hosts, and Exadata and it components assessed for compliance, highlighting the scope of compliance monitoring across the IT environment.

    **Violation and Errors:**  A pie chart showing compliant, warning, and non-compliant statuses, highlighting the proportion of violations and errors for quick identification of areas needing attention.

    **Open Security Incidents numbers:** Displays current open security incidents, including affected targets, severity, and remediation status for timely resolution and prioritization.

    **Compliance Standard score Distribution:** Shows the distribution of compliance scores across different targets or policies, highlighting overall compliance health and areas needing improvement.

    **Compliance Violations by Target type:** Breaks down violations by target type (e.g., databases, hosts, Exadata), identifying problematic areas and pinpointing systems prone to compliance issues.

    **Compliance Summary:** Provides an overview of Targets Compliance status, key metrics about violations, average compliance scores, and comprehensive Report, facilitating targeted efforts to meet compliance standards.

7. You will see Compliance Summary, Standards tab

    ![Compliance Summary](images/compliance-summary.png  " ")

    Displays number of Compliant Targets, Non-Compliant Targets, Critical, Warning, Minor Warning and Average Compliance Score.

    Click on Non-Compliant Targets number, pop-up window shows Targets showing the Compliance Score.

    ![Compliance Summary](images/non-compliant-target.png " ")

    Click on 'x' to close.

8. Let us analyze the severity

    Click on **Critical** number, you will see unique violations for this target.

    ![Compliance Summary](images/non-compliant-target-window-critical.png " ")

    Expand the name hr.subnet.vcn.oraclevcn.com_FINANCE caret-right ![Caret-right](images/cis-caret-search.png " ") icon.

    You will see the details of the evaluation.

    You will get the comprehensive view of the violation.

    ![Compliance Summary](images/non-compliant-target-critical-detail-window.png " ")

    Click on 'x' to close.

    Similarly, we can explore Warning and Minor Warning violations if they exist.

9. Let us drill down to results.

    Click on **Standards >> Name** and let us remediate one of the solution.

    You will see "97" violations number going down to "96".

    ![Navigate to result](images/compliance-standard-summary-violation.png " ")

    You will be presented with the **Compliance Results**, including the target violations against the selected standard, as well as its score and last evaluation date.

    You will gain insight into CIS standard main categories, controls, and their corresponding rule violations.

    The Target Scorecard pie chart displays the overall compliance evaluation status of the monitored target, summarizing its adherence to defined CIS benchmark security policies.

    The Rule Evaluations pie chart represents a summary of each rule's evaluation status—compliant, critical, warning, minor warning, and error—in terms of the percentage of rules.

    ![Validations](images/cis-compliance-pdb-controls.png " ")

    Results by Target tab shows number of violations occurred, score and latest target evaluated date.

    ![Overview scorecard](images/cis-compliance-pdbresult.png " ")

    Let us click on Violations number.

10. In the **Violations** window, you will find failed Compliance Standard rule description along with the Violation Count.

    ![Validate CIS ](images/cis-compliance-rule-violation-count.png " ")

    Click on Violation Count.

    You can export to Excel for off line Analysis.

    ![Validate CIS ](images/cis-compliance-rule-violation.png " ")

    Click on 'Back'

    ![Validate CIS](images/cis-violations-close.png " ")

    Click on 'Close'

11. Let us select **Violations** tab, you'll find the table with comprehensive details for each rule violations for the target and severity of the violation.

    ![Validate CIS ](images/cis-compliance-pdb-violations.png " ")

    Click on Summary tab.

12. Click on caret-right ![Caret-right](images/cis-caret-search.png " ") icon.

    Explore each main category controls' **Violations**.

    Let us select violation **Ensure 'DBA' Is Revoked from Unauthorized 'GRANTEE'**.  

    ![Check for violations](images/cis-rule-violation.png " ")

13. You will notice that the "Unauthorized user 'IX' has been assigned the DBA role".

    The Oracle DBA role grants powerful administrative privileges for configuration changes and access to sensitive information. 'IX' should not have this role.

    ![Check for violations](images/cis-unauthorized-grantee.png " ")

    Click **Close**.

14. Click on the **Rule Details** tab.

    You will find detailed rule information including severity and recommended remediation.

    ![Check for violations](images/cis-rule-details.png " ")

15. Click on the **Violation Events** tab.

    This will provide you essential information about violation details about user, who has a elevated DBA privilege, which should be revoked.

    ![Rule Details](images/cis-violation-events.png  " ")

    To access the table row section, click on it.

16. You will find **Event details** on violated rule information, violation details, and a guided resolution option for recommendations.

    ![Event Details](images/cis-event-details-violation.png  " ")

    Click on **Corrective actions**.

    You will see the **Corrective Actions** pop-up window.

    ![Event Details](images/corrective-actions-pop-up-window.png  " ")

    Don't forget to select the row labeled **CORRECTIVE_ACTION_REVOKE_DBA_ROLE_PRIVILEGE**.

17. You will find the **Database Credentials** and **Host Credentials**.

    ![Event Details](images/preferred-credentials-selection.png  " ")

    Remember to choose **Preferred** for both credentials and then click **Submit**.

18. You will be presented with a pop-up window for  **Corrective action**.

    ![Event Details](images/corrective-action-submitt.png " ")

    Click to view execution details.  

19. You will notice that the DBA role job was successfully revoked upon refreshing the icon![Refresh](images/refresh-icon.png "  ")  ***in Jobs page***

    ![Event Details](images/dba_grantee_revoke_job.png  " ")

20. Now, Navigate to ***Targets >> Databases***.

    ![Navigate to result](images/target-databases.png  " ")

21. You will see the **hr.subnet.vcn.oraclevcn.com_FINANCE** PDB in the Name.

    ![Navigate to result](images/select-pluggable-pdb.png  " ")

    Click on the selected PDB, you will be navigated to the PDB database page.

    ![Navigate to result](images/dbs-pdb-page.png " ")

22. Now, Click on ***Oracle Database >> Configuration >> Latest***.

    ![Navigate to result](images/db-configuration-latest.png  " ")

    You will see the latest configuration.

    ![Latest Configuration](images/pdb-latest-configuration.png  " ")

    Click **Refresh**.

23. **Processing** will take about few minutes to complete and get refreshed.

    ![Latest Configuration](images/configuration-refresh-pdb.png  " ")

24. Now, Navigate to ***Enterprise >> Compliance >> Results***.

    ![Latest Configuration](images/navigate-results.png  " ")

25. You will be directed to the **Compliance Results** page, where you will notice a reduction in violations from **97 to 96**.

    ![Compliance Results](images/cis-compliance-violation-results.png " ")

    Click on Compliance Standards **Oracle 19c Database CIS 1.1.0** Results.

26. You will see the details of the **Compliance Standards Results**.

    Verify the selected violation: **Ensure that 'DBA' is revoked from unauthorized 'GRANTEE'**.

    The violation count is currently "0".

    ![Compliance Results](images/cis-pdb-compliance-remeidiated.png  " ")

    "No data to display" appears when you click on the **Violations Events tab**.

    ![Compliance Results](images/cis-pdb-no-data.png  " ")

    Thus, Ensure 'DBA' Is Revoked from Unauthorized 'GRANTEE' " (Compliance Standard Rule) violation have been successfully remediated through corrective action.

    Click on Summary tab.

27. Now, Navigate to ***Enterprise >> Compliance >> Dashboard***

    ![Compliance Results](images/compilance-dashboard-option.png  " ")

    **The Dashboard view provides a reduced number of violations count from 97 to 96** in the Compliance Summary.

    ![Compliance Dashboard](images/compliance-dashboard-counts.png " ")

28. Click on **Report** to generate a comprehensive compliance report for CIS compliance standards and its associated pluggable database targets.

    ![Compliance Report](images/cis-standard-detaiil-report1.png  " ")

    Towards the bottom of the page in the Compliance Summary section, click on the report against each Compliance standard.

    Sample report, which will show Summary of Pass and Failed Rules, Compliance Score, Results Details along with Standard Rules.

    ![Compliance Report](images/cis-standard-report2.png  " ")
    ![Compliance Report](images/cis-standard-report3.png " ")
    ![Compliance Report](images/cis-standard-report4.png " ")

    This completes the Automated Database Patching at Scale with Fleet Maintenance HUB and Oracle 19c Database CIS V1.1.0 - Level 1 - RDBMS benchmark using Unified Auditing for Oracle Pluggable Database benchmark Rule Violation Remediation by corrective Action.

    ![Compliance Dashboard](images/dashboard-setup-logout.png " ")

    Click on ***Setup >> Log Out***  

That completes the Database Patching and Compliance lab.

You may now proceed to the next lab.

## Learn More
  - [Oracle Enterprise Manager](https://www.oracle.com/enterprise-manager/)
  - [Oracle Enterprise Manager Fleet Maintenance](https://www.oracle.com/manageability/enterprise-manager/technologies/fleet-maintenance.html)
  - [Enterprise Manager Documentation Library](https://docs.oracle.com/en/enterprise-manager/index.html)
  - [Database Lifecycle Management](https://docs.oracle.com/en/enterprise-manager/cloud-control/enterprise-manager-cloud-control/13.5/lifecycle.html)
  - [Database Cloud Management](https://docs.oracle.com/en/enterprise-manager/cloud-control/enterprise-manager-cloud-control/13.5/cloud.html)
  - [Oracle Critical Patch Updates, Security Alerts and Bulletins](https://www.oracle.com/in/security-alerts/)
  - [Enterprise Manager: How to automate corrective actions to remediate CIS Compliance rule violations](https://www.youtube.com/watch?v=gskVyH0oNeg)

## Acknowledgements
  - **Authors**
    - Romit Acharya, Oracle Enterprise Manager Product Management
    - Shiva Prasad, Oracle Enterprise Manager Product Management
  - **Last Updated By/Date** -Romit Acharya, Oracle Enterprise Manager Product Management, May 2024
