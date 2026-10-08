
## Experiment 1: Installation and Setup of Ubuntu in VirtualBox

**Aim:** Create a virtual machine in VirtualBox and install Ubuntu on it.

### Steps
1. Download and install **Oracle VirtualBox** on your computer.
2. Download the **Ubuntu ISO** file.
3. Open VirtualBox and click **New**.
4. Enter a VM name, set the type to **Linux**, and select the downloaded ISO file.
5. Set the RAM to **4 GB** and assign **2 CPU cores**.
6. Create a virtual hard disk of **25 GB**.
7. Click **Start** to power on the virtual machine.
8. Follow the on-screen instructions to complete the Ubuntu installation.
9. Restart the VM and log in.
10. Install **Guest Additions** to enable full-screen support.

**Result:** Ubuntu is installed and running inside VirtualBox.

---

## Experiment 2: Network Setup and Running a C Program in Ubuntu

**Aim:** Confirm the VM has network access, install the GCC compiler, and compile and run a C program.

### Steps
1. Go to the VM's **Settings → Network** and make sure **Adapter 1** is enabled.
2. Start the VM and open a terminal (`Ctrl + Alt + T`).
3. Update the package list:
   ```bash
   sudo apt update
   ```
4. Install the GCC compiler and build tools:
   ```bash
   sudo apt install build-essential
   ```
5. Check the compiler version:
   ```bash
   gcc --version
   ```
6. Create and open a new file:
   ```bash
   nano hello.c
   ```
7. Write the following code:
   ```c
   #include <stdio.h>

   int main() {
       printf("Hello");
       return 0;
   }
   ```
8. Save and exit nano (`Ctrl + O`, `Enter`, `Ctrl + X`).
9. Compile the program:
   ```bash
   gcc hello.c -o hello
   ```
10. Run it:
    ```bash
    ./hello
    ```

**Result:** The terminal prints `Hello`.