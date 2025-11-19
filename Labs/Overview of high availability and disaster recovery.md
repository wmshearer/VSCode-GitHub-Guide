# Exercise 1 — Task 1: Design the appropriate high-availability and disaster-recovery solution

- [ ] Review the scenario describing Contoso’s recent disaster event, business requirements, and goals.

- [ ] Record answers to the question:  
      **“What actions should you take and which technologies should you consider?”**

- [ ] Recommended actions and technologies:

    - Create a **Business Recovery Plan** that prioritizes divisional and service requirements, with **customer-facing financial systems** being the most critical.

    - Use **Live Migration** to support monthly planned downtime with **zero service interruption** for virtual machines.

    - Use **Storage Migration** to move VM storage away from legacy storage, upgrade storage hardware, and move storage back — **without VM downtime**.

    - Use **Hyper-V Recovery Manager** integrated with **Hyper-V Replica** to provide a reliable **disaster recovery** solution for critical virtual machines across site locations.

- [ ] Confirm that the proposed design meets the requirements:

    - < 1 minute RTO for the Finance division  
    - Zero data loss RPO for financial transactions  
    - Support for scaling and growth  
    - No downtime during updates or storage replacement  
    - Cost-efficient alignment with Hyper-V and Windows Server 2022 capabilities

### Exercise 2 — Task 1: Configure and perform storage migration

- [ ] Sign in to **LON-SVR2** as **Contoso\Administrator** with the password:

    ```powershell
    Pa55w.rd
    ```

- [ ] On the taskbar, open **Hyper-V Manager**.

- [ ] In **Hyper-V Manager**, select **LON-SVR2** in the navigation pane  
      to display its VMs in the **Virtual Machines** section.

- [ ] In the **Virtual Machines** list, right-click **LON-SVR5**, then select **Start**  
      to ensure the VM is running for live storage migration testing.

- [ ] Right-click **LON-SVR5** again and select **Settings**.

- [ ] In **Settings for LON-SVR5**, under **IDE Controller 0**, select **Hard Drive**.

- [ ] Verify that the VHD file is located at:

    ```powershell
    E:\Labfiles\LON-SVR5
    ```

- [ ] Select **OK** to close the VM settings.

- [ ] In **Hyper-V Manager**, right-click **LON-SVR5**, then select **Move**.

- [ ] In the *Move "LON-SVR5"* wizard, on *Before You Begin*, select **Next**.

- [ ] On *Choose Move Type*, select  
      **Move the virtual machine's storage**, then select **Next**.

- [ ] On *Choose Options for Moving Storage*, select  
      **Move all of the virtual machine's data to a single location**, then select **Next**.

- [ ] On *Choose a new location for virtual machine*, enter:

    ```powershell
    C:\VMs\LON-SVR5
    ```

    Then select **Next**.

- [ ] On the *Completing Move Wizard* page, select **Finish**.

- [ ] A dialog appears showing **Performing the move**.  
      While it completes, connect to **LON-SVR5** and verify the VM remains accessible  
      (this confirms the move occurs **without downtime**).

- [ ] After the move finishes, right-click **LON-SVR5** again and select **Settings**.

- [ ] Under **IDE Controller 0 → Hard Drive**, confirm that:

    ```powershell
    LON-SVR5.vhd
    ```

    is now stored under:

    ```powershell
    C:\VMs\LON-SVR5
    ```

- [ ] Select **OK** to close the settings and confirm that storage migration succeeded.

### Exercise 3 — Task 1: Enable replication on both host machines

- [ ] Sign in to **LON-SVR3** as **Contoso\Administrator** using the password:

    ```powershell
    Pa55w.rd
    ```

- [ ] On the taskbar, open **Hyper-V Manager**.

- [ ] In **Hyper-V Manager**, right-click **LON-SVR3**, then select **Hyper-V Settings**.

- [ ] In **Hyper-V Settings for LON-SVR3**, select **Replication Configuration**.

- [ ] Check **Enable this computer as a Replica server**.

- [ ] In **Authentication and ports**, select:

    ```
    Use Kerberos (HTTP)
    ```

- [ ] In **Authorization and storage**, select:

    ```
    Allow replication from any authenticated server
    ```

- [ ] Select **Browse**, expand **This PC**, expand **Local Disk (C:)**, then create a new folder:

    - Name: **VMReplica**

- [ ] Select the folder:

    ```powershell
    C:\VMReplica
    ```

    then select **Select Folder**.

- [ ] In **Hyper-V Settings for LON-SVR3**, select **OK**.

- [ ] In the warning dialog, select **OK**.

- [ ] Select **Start**, then open **Windows Defender Firewall**.

- [ ] Select **Advanced settings**.

- [ ] Select **Inbound Rules**.

- [ ] Locate the rule:

    ```
    Hyper-V Replica HTTP Listener (TCP-In)
    ```

- [ ] Right-click the rule and select **Enable Rule**.

- [ ] Close the **Windows Defender Firewall with Advanced Security** console.

- [ ] Close **Windows Defender Firewall**.

- [ ] Switch to **LON-SVR2**.

- [ ] Repeat **all steps above** (steps 1 through 16) for LON-SVR2.

### Exercise 3 — Task 2: Configure replication for LON-SVR5 VM

- [ ] On **LON-SVR2**, open **Hyper-V Manager**.

- [ ] In the **Virtual Machines** list, right-click **LON-SVR5**, then select **Enable Replication**.

- [ ] In the *Enable Replication for LON-SVR5* wizard, on *Before You Begin*, select **Next**.

- [ ] On *Specify Replica Server*, enter:

    ```powershell
    LON-SVR3
    ```

    Then select **Next**.

- [ ] On *Specify Connection Parameters*, select:

    ```
    Use Kerberos authentication (HTTP)
    ```

    Then select **Next**.

- [ ] On *Choose Replication VHDs*, select **Next**.

- [ ] On *Configure Replication Frequency*, choose:

    ```
    30 seconds
    ```

    Then select **Next**.

- [ ] On *Configure Additional Recovery Points*, select:

    ```
    Maintain only the latest recovery point
    ```

    Then select **Next**.

- [ ] On *Choose Initial Replication Method*, select:

    - **Send initial copy over the network**  
    - **Start replication immediately**

    Then select **Next**.

- [ ] On the *Completing the Enable Replication Wizard* page, select **Finish**.

- [ ] Wait approximately **five minutes** for the initial replication to complete.  
      Monitor progress in the **Status** column in Hyper-V Manager.

- [ ] When replication reaches **100%**, switch to **LON-SVR3**.

- [ ] In **Hyper-V Manager** on LON-SVR3, confirm that **LON-SVR5** appears in an:

    ```
    Off
    ```

    state.


### Exercise 3 — Task 3: Validate a planned failover to the replica site

- [ ] On **LON-SVR2**, open **Hyper-V Manager**.

- [ ] Right-click **LON-SVR5**, select **Replication**, then select **View Replication Health**.

- [ ] Review the **Replication Health** window and ensure that **no errors** are reported.

- [ ] Select **Close** to exit the Replication Health window.

- [ ] Right-click **LON-SVR5** again, then select **Shut Down**.

- [ ] Wait for **LON-SVR5** to fully shut down.  
      *(A planned failover requires the VM to be off.)*

- [ ] Right-click **LON-SVR5**, point to **Replication**, then select **Planned Failover**.

- [ ] In the **Planned Failover** window, verify the option is selected:

    ```
    Start the Replica virtual machine after failover
    ```

- [ ] Select **Fail Over** to initiate the planned failover.

- [ ] Switch to **LON-SVR3**.

- [ ] In **Hyper-V Manager** on LON-SVR3, verify that **LON-SVR5** is now **running** on the replica host.