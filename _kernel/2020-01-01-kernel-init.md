---
layout: post
title: kernel Head.S 
tags: 
- text
---

# 0. Head.S

<div class="mermaid"> 
flowchart TD; 
  subgraph Head.S;
    direction TB;
    subgraph HEAD
    primary_entry
    end
    subgraph primary_entry.
    preserve_boot_args-->init_kernel_el;
    init_kernel_el-->MIN_KIMG_ALIGN;
    MIN_KIMG_ALIGN-->set_cpu_boot_mode_flag;
    set_cpu_boot_mode_flag-->__create_page_tables;
    __create_page_tables-->__cpu_setup;
    __cpu_setup-->primary_switch;
    end
    primary_entry-->primary_entry.
  end
</div>



# 1.HEAD


~~~
__HEAD
    efi_signature_nop       
    b       primary_entry
    .quad   0    
    le64sym _kernel_size_le 
    le64sym _kernel_flags_le  
    .quad   0   
    .quad   0             
    .quad   0             
    .ascii  ARM64_IMAGE_MAGIC   
    .long   .Lpe_header_offset  
__EFI_PE_HEADER
__INIT
~~~


# 2. primary_entry

<div class="mermaid"> 
  flowchart TD; 
  subgraph preserve_boot_args
  부트_파라미터_저장;
  end
  preserve_boot_args-->init_kernel_el;
  subgraph init_kernel_el
  el2로_진입했을경우_el2_설정;
  end
  init_kernel_el-->__PHYS_OFFSET;
  subgraph __PHYS_OFFSET
  KERNEL_START지점;
  end
  __PHYS_OFFSET-->MIN_KIMG_ALIGN;
  subgraph MIN_KIMG_ALIGN;
  KASLR_OFFSET;
  end
  MIN_KIMG_ALIGN-->set_cpu_boot_mode_flag;
  subgraph set_cpu_boot_mode_flag
  CPU_boot_mode_저장
  end
  set_cpu_boot_mode_flag-->__create_page_tables;
  subgraph __create_page_tables
  initial_page_table_저장
  end
  __create_page_tables-->__cpu_setup;
  subgraph __cpu_setup
  initialise_processor;
  end
  __cpu_setup-->__primary_switch;
  subgraph __primary_switch
  MMU_enable
  end
</div>

~~~
SYM_CODE_START(primary_entry)
    bl      preserve_boot_args
    bl      init_kernel_el       
    adrp    x23, __PHYS_OFFSET
    and     x23, x23, MIN_KIMG_ALIGN - 1
    bl      set_cpu_boot_mode_flag
    bl      __create_page_tables
    bl      __cpu_setup             
    b       __primary_switch
SYM_CODE_END(primary_entry)
~~~


# 3. preserve_boot_args

~~~
SYM_CODE_START_LOCAL(preserve_boot_args)
    mov     x21, x0 
    adr_l   x0, boot_args 
    stp     x21, x1, [x0] 
    stp     x2, x3, [x0, #16]
    dmb     sy  
    add     x1, x0, #0x20 
    b       dcache_inval_poc
SYM_CODE_END(preserve_boot_args)
~~~