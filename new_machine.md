## add new machine in qemu -M 

### checkout to tag v10.0.2
add the following two parts code into hw/aspeed.c 

```c
 {
        .name          = MACHINE_TYPE_NAME("custom-evb"),
        .parent        = MACHINE_TYPE_NAME("ast2600-evb"),  //orig : TYPE_ASPEED_MACHINE,
        .class_init    = aspeed_machine_custom_evb_class_init,
 },
```

```C 
static void aspeed_machine_custom_evb_class_init(ObjectClass *oc, void *data)
{
    MachineClass *mc = MACHINE_CLASS(oc);
    AspeedMachineClass *amc = ASPEED_MACHINE_CLASS(oc);

    mc->desc       = "Aspeed AST2600 EVB (Cortex-A7)";
    amc->soc_name  = "ast2600-a3";
    amc->hw_strap1 = AST2600_EVB_HW_STRAP1;
    amc->hw_strap2 = AST2600_EVB_HW_STRAP2;
    amc->fmc_model = "mx66u51235f";
    amc->spi_model = "mx66u51235f";
    amc->num_cs    = 1;
    amc->macs_mask = ASPEED_MAC0_ON | ASPEED_MAC1_ON | ASPEED_MAC2_ON |
                     ASPEED_MAC3_ON;
    amc->sdhci_wp_inverted = true;
    amc->i2c_init  = ast2600_evb_i2c_init;
    mc->auto_create_sdcard = true;
    mc->default_ram_size = 1 * GiB;
    aspeed_machine_class_init_cpus_defaults(mc);
    //aspeed_machine_ast2600_class_emmc_init(oc);
};
```

use aspeed 2600 image to verify 
```
../build/qemu-system-arm -M acustom-evb -drive format=raw,file=obmc-phosphor-image-ast2600-default.static.mtd,if=mtd -nographic
```
