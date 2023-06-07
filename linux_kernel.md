# 1. hello word

- makefile
```makefile
obj-m += mydriver.o

all:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules
clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

- mydriver.c
```c
#include <linux/init.h>
#include <linux/module.h>
#include <linux/kernel.h>

MODULE_LICENSE("GPL");
MODULE_AUTHOR("abin");
MODULE_DESCRIPTION("A simple example Linux module.");
MODULE_VERSION("0.01");

static int __init lkm_example_init(void)
{
    printk(KERN_INFO "Hello, World!\n");
    return 0;
}

static void __exit lkm_example_exit(void)
{
    printk(KERN_INFO "Goodbye, World!\n");
}

module_init(lkm_example_init);
module_exit(lkm_example_exit);
```

# 2. open kernel dynamic debug logging
like dev_err, dev_warn print
1. configure kernel compile option, enable DYNAMIC_DEBUG.[this option is enabled in most released kernel]
   - open kernel config file, maybe at /usr/src/linux/.config或/boot/config-*
   - search option CONFIG_DYNAMIC_DEBUG, ensure the value is "y" or "m". that means dynamic debug was enabled.
   - add the following to the config file
      ```kconfig
      CONFIG_DYNAMIC_DEBUG=y
      CONFIG_DYNAMIC_DEBUG_CORE=y
      CONFIG_DYNAMIC_DEBUG_TOOL=y
      ```

2. enable dynamic debug logging
   
   `echo 'module <module_name> +p' > /sys/kernel/debug/dynamic_debug/control`

   if you want debug usbcore, jsut need replace "<module_name>" to "usbcore".