

#### 启动容器

```shell
> docker run --privileged -it ipking/php-gdb:7.0.0
[root@3eb5c75ac437 home]#
```


#### 添加t.php

```php
<?php
$str = 'Z';
$str++;
echo $str;
```

#### 启动 gdb

```shell
# 启动 gdb 并调试 php
[root@3eb5c75ac437 home]# gdb php
GNU gdb (GDB) Red Hat Enterprise Linux 8.2-16.el8
Copyright (C) 2018 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-redhat-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from php...done.
(gdb)
```

#### 设置 breakpoint

```shell
(gdb) b zend_vm_get_opcode_handler
Breakpoint 1 at 0x8d0877: file /home/php-7.0.0/Zend/zend_vm_execute.h, line 49692.
```

#### 保存 breakpoint

```shell
(gdb) save breakpoints /home/php-breakpoints.txt
Saved to file '/home/php-breakpoints.txt'.
```

#### 加载 breakpoint

```shell
(gdb) source /home/php-breakpoints.txt
Breakpoint 2 at 0x8d0877: file /home/php-7.0.0/Zend/zend_vm_execute.h, line 49692.
```


#### 调试t.php

```shell
(gdb) r t.php
Starting program: /usr/local/bin/php t.php
warning: Error disabling address space randomization: Operation not permitted
Missing separate debuginfos, use: yum debuginfo-install glibc-2.28-164.el8.x86_64
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib64/libthread_db.so.1".

Breakpoint 1, zend_vm_get_opcode_handler (opcode=149 '\225', op=0x1060420 <executor_globals+864>) at /home/php-7.0.0/Zend/zend_vm_execute.h:49692
49692                   return zend_opcode_handlers[opcode * 25 + zend_vm_decode[op->op1_type] * 5 + zend_vm_decode[op->op2_type]];
Missing separate debuginfos, use: yum debuginfo-install libxcrypt-4.1.1-6.el8.x86_64 libxml2-2.9.7-9.el8_4.2.x86_64 xz-libs-5.2.4-3.el8.x86_64 zlib-1.2.11-17.el8.x86_64
(gdb) c
Continuing.

Breakpoint 1, zend_vm_get_opcode_handler (opcode=149 '\225', op=0x1060440 <executor_globals+896>) at /home/php-7.0.0/Zend/zend_vm_execute.h:49692
49692                   return zend_opcode_handlers[opcode * 25 + zend_vm_decode[op->op1_type] * 5 + zend_vm_decode[op->op2_type]];
(gdb)
Continuing.

Breakpoint 1, zend_vm_get_opcode_handler (opcode=149 '\225', op=0x1060460 <executor_globals+928>) at /home/php-7.0.0/Zend/zend_vm_execute.h:49692
49692                   return zend_opcode_handlers[opcode * 25 + zend_vm_decode[op->op1_type] * 5 + zend_vm_decode[op->op2_type]];
(gdb)
Continuing.

Breakpoint 1, zend_vm_get_opcode_handler (opcode=158 '\236', op=0x1060688 <executor_globals+1480>) at /home/php-7.0.0/Zend/zend_vm_execute.h:49692
49692                   return zend_opcode_handlers[opcode * 25 + zend_vm_decode[op->op1_type] * 5 + zend_vm_decode[op->op2_type]];
(gdb)
Continuing.

Breakpoint 1, zend_vm_get_opcode_handler (opcode=38 '&', op=0x7fa4ba4720c0) at /home/php-7.0.0/Zend/zend_vm_execute.h:49692
49692                   return zend_opcode_handlers[opcode * 25 + zend_vm_decode[op->op1_type] * 5 + zend_vm_decode[op->op2_type]];
(gdb) fin
Run till exit from #0  zend_vm_get_opcode_handler (opcode=38 '&', op=0x7fa4ba4720c0) at /home/php-7.0.0/Zend/zend_vm_execute.h:49692
0x00000000008d091f in zend_vm_set_opcode_handler (op=0x7fa4ba4720c0) at /home/php-7.0.0/Zend/zend_vm_execute.h:49697
49697           op->handler = zend_vm_get_opcode_handler(zend_user_opcodes[op->opcode], op);
Value returned is $1 = (const void *) 0x8b0f65 <ZEND_ASSIGN_SPEC_CV_CONST_HANDLER>
(gdb) c
Continuing.

Breakpoint 1, zend_vm_get_opcode_handler (opcode=36 '$', op=0x7fa4ba4720e0) at /home/php-7.0.0/Zend/zend_vm_execute.h:49692
49692                   return zend_opcode_handlers[opcode * 25 + zend_vm_decode[op->op1_type] * 5 + zend_vm_decode[op->op2_type]];
(gdb) fin
Run till exit from #0  zend_vm_get_opcode_handler (opcode=36 '$', op=0x7fa4ba4720e0) at /home/php-7.0.0/Zend/zend_vm_execute.h:49692
0x00000000008d091f in zend_vm_set_opcode_handler (op=0x7fa4ba4720e0) at /home/php-7.0.0/Zend/zend_vm_execute.h:49697
49697           op->handler = zend_vm_get_opcode_handler(zend_user_opcodes[op->opcode], op);
Value returned is $2 = (const void *) 0x8a83fb <ZEND_POST_INC_SPEC_CV_HANDLER>
(gdb) b ZEND_POST_INC_SPEC_CV_HANDLER
Breakpoint 2 at 0x8a8403: file /home/php-7.0.0/Zend/zend_vm_execute.h, line 28181.
(gdb) c
Continuing.

Breakpoint 1, zend_vm_get_opcode_handler (opcode=70 'F', op=0x7fa4ba472100) at /home/php-7.0.0/Zend/zend_vm_execute.h:49692
49692                   return zend_opcode_handlers[opcode * 25 + zend_vm_decode[op->op1_type] * 5 + zend_vm_decode[op->op2_type]];
(gdb)
Continuing.

Breakpoint 1, zend_vm_get_opcode_handler (opcode=40 '(', op=0x7fa4ba472120) at /home/php-7.0.0/Zend/zend_vm_execute.h:49692
49692                   return zend_opcode_handlers[opcode * 25 + zend_vm_decode[op->op1_type] * 5 + zend_vm_decode[op->op2_type]];
(gdb)
Continuing.

Breakpoint 1, zend_vm_get_opcode_handler (opcode=62 '>', op=0x7fa4ba472140) at /home/php-7.0.0/Zend/zend_vm_execute.h:49692
49692                   return zend_opcode_handlers[opcode * 25 + zend_vm_decode[op->op1_type] * 5 + zend_vm_decode[op->op2_type]];
(gdb)
Continuing.

Breakpoint 2, ZEND_POST_INC_SPEC_CV_HANDLER () at /home/php-7.0.0/Zend/zend_vm_execute.h:28181
28181           var_ptr = _get_zval_ptr_cv_undef_BP_VAR_RW(execute_data, opline->op1.var);
(gdb) bt
#0  ZEND_POST_INC_SPEC_CV_HANDLER () at /home/php-7.0.0/Zend/zend_vm_execute.h:28181
#1  0x000000000086ea1d in execute_ex (ex=0x7fa4ba414030) at /home/php-7.0.0/Zend/zend_vm_execute.h:414
#2  0x000000000086eb2e in zend_execute (op_array=0x7fa4ba47c000, return_value=0x0) at /home/php-7.0.0/Zend/zend_vm_execute.h:458
#3  0x0000000000814b35 in zend_execute_scripts (type=8, retval=0x0, file_count=3) at /home/php-7.0.0/Zend/zend.c:1428
#4  0x0000000000787277 in php_execute_script (primary_file=0x7fff30e1f380) at /home/php-7.0.0/main/main.c:2471
#5  0x00000000008d24b0 in do_cli (argc=2, argv=0x2e4a520) at /home/php-7.0.0/sapi/cli/php_cli.c:974
#6  0x00000000008d3481 in main (argc=2, argv=0x2e4a520) at /home/php-7.0.0/sapi/cli/php_cli.c:1345
(gdb)
```
