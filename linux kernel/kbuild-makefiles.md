内核Makefile的说明位于如下路径：  
```bash
Documentation/kbuild/
```
以kernel Makefile中调用的 as-instr为例，可以查询该接口的含义为：
```bash
    as-instr
        as-instr checks if the assembler reports a specific instruction
        and then outputs either option1 or option2
        C escapes are supported in the test instruction
        Note: as-instr-option uses KBUILD_AFLAGS for $(AS) options
```
