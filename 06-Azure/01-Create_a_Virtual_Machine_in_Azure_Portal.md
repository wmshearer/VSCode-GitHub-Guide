
# Create a Virtual Machine in the Azure Portal

## Exercise — Task 1: Create the virtual machine

- [ ] Sign in to the Azure portal:

    ```
    https://portal.azure.com
    ```

- [ ] From the **All services** menu, search for:

    ```
    Virtual machines
    ```

    then select it.

- [ ] Select:

    ```
    + Create → + Azure Virtual machine
    ```

---

### Configure the **Basics** tab

- [ ] Subscription → **Use default supplied**

- [ ] Resource group → **Use default supplied** (from dropdown)

- [ ] Virtual machine name:

    ```
    myVM
    ```

- [ ] Region:

    ```
    (US) East US
    ```

- [ ] Availability options:

    ```
    No infrastructure redundancy options required
    ```

- [ ] Image → select:

    - **See all images**
    - Search for:

        ```
        Windows Server 2019 Datacenter - Gen2
        ```

- [ ] Size:

    ```
    Standard D2s v3
    ```

- [ ] Administrator username:

    ```
    azureuser
    ```

- [ ] Administrator password:

    ```
    Pa$$w0rd1234
    ```

---

### Inbound port rules

- [ ] Select:

    ```
    Allow selected ports
    ```

- [ ] Select inbound ports:

    - **RDP (3389)**
    - **HTTP (80)**

---

### Networking tab

- [ ] Confirm inbound ports:

    - **RDP (3389)**
    - **HTTP (80)**

are selected under **Select inbound ports**.

---

### Monitoring tab

- [ ] Set **Boot diagnostics** to:

    ```
    Disable
    ```

---

### Finalize VM creation

- [ ] Select **Review + create**.

- [ ] Wait for **Validation passed**.

- [ ] Select **Create**.

- [ ] Wait 5–7 minutes for deployment to finish (watch bell icon notifications).
