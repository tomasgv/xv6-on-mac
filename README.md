# xv6-on-Mac
Tutorial on how to compile XV6 on a Mac using [Lima](https://github.com/lima-vm/lima). Quick setup, launch and debug using gdb-multiarch. Yes, you can use it with ARM processors.

Specially made for the Operative Systems course taught at CentraleSupélec.


## Setup
[Homebrew](https://brew.sh) is required.

### Lima installation

We'll install it through homebrew.
  ```console
  $ brew install lima
  ```

You can find more information about lima [here](https://github.com/lima-vm/lima).

### VM setup

1. Quick way to create a default instance with x86_64 architecture:
    
    ```console
    $ limactl start --set='.arch = "x86_64"'
    ```
  
    Another example of default instance with modified parameters:

    ```console
    $ limactl start --set='.arch = "x86_64" | .memory="4GiB" | disk="10GiB"'
    ```
    You can also modify the `default.yaml` file in `.lima/_config` or use the file provided. 

2. After this, you can connect to the VM using:
   
    ```console
    $ lima
    ```
    You can stop the VM using:

    ```console
    $ limactl stop default
    ```

    And start it again with:
    ```console
    $ limactl start default
    ```



### XV6 compilation

Before starting, make sure your VM has **write** access on the folder that will be used to compile XV6. If you used the default configuration, the writeable folder will be `/tmp/lima/`. You can modify this in the configuration file (look for `mounts`).

1. You'll have to install the requirements to emulate the machine:

    ```console
    $ apt install -y build-essential gdb-multiarch qemu-system-misc gcc-riscv64-linux-gnu binutils-riscv64-linux-gnu
    ```

    We also install gdb-multiarch here for debugging purposes.

2. **Done!** Now you can run `make qemu` to run xv6 or `make qemu-gdb` to run it on debug mode:

    ```console
    $ make qemu
    ```

## Debugging with GDB

In this tutorial we'll install `gdb-multiarch` and use it to debug.
1. Connect to your VM:
   ```console
   $ lima
   ```

2. If you haven't installed `gdb-multiarch` already, run:
    ```console
    $ sudo apt-get install gdb-multiarch 
    ```

1. Compile xv6 in debug mode:
   ```console
   $ make qemu-gdb
   ```
2. In another terminal (in the VM) run the debugger:
   ```console
   $ gdb-multiarch
   ```
## Connecting to the VM through SSH in VSCode

In the case you want to connect to your VM using VSCode (e.g. to use Native Debug with it) you'll want to connect to it using ssh.

1. You can dump the config needed to connect to your VM:
  
    ```console
    $ limactl show-ssh --format=config default > ./ssh-config
    ```
  
    This will generate a `ssh-config` file. 

2. Now you can connect to your VM:
  
    ```console
    $ ssh -F ./ssh-config lima-default
    ```
    You can use this command to connect from VSCode (through the blue button on the bottom-left corner)

    Every new terminal you open in VSCode will be already connected to the VM :)

## Annex: Study resources

If you're having trouble studying OS, I recommend you take a look into [hhp3's xv6 kernel Playlist](https://www.youtube.com/watch?v=fWUJKH0RNFE&list=PLbtzT1TYeoMhTPzyTZboW_j7TPAnjv9XB&index=1) or [YehudaShapira's notes on xv6](https://github.com/YehudaShapira/xv6-explained/tree/master).
