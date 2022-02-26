---
layout: post
title: kernel device tree
tags: 
- text
---



sm8250 기준으로 작성한 문서입니다.
root 

aliases
 i2c 20
 spi 20


chosen

clocks
    xo_board
    sleep_clk  
cpus
    cpu 8
    cpu-map 
        cluster8
    firmware
        scm
    memory
    pmu
    psci
    reserver-memory
        hyp_mem
        xbl_aop_mem
        cmd_db
        smem_mem
        remover_mem
        camera_mem
        wlan_mem
        ipa_fw_mem
        ipa_gsi_mem
        gpu_mem
        npu_mem
        video_mem
        cvp_mem
        cdsp_mem
        slpi_mem
        adsp_mem
        spss_mem
        cdsp_secure_heap
    smem
    smp2p-adsp
    smp2p-cdsp
    smp2p_slpi
    soc
        gcc
        ipcc
        rng
        qup_opp_table
        gpi_dma2
        qupv3_id_2
            i2c14
            spi14
            i2c15
            spi15
            i2c16
            spi16
            i2c17
            spi17
            uart17
            i2c18
            spi18
            uart18
            i2c19
            spi19
    gpi_dma0 

