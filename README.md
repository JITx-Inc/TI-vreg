# Installation

In slm.toml add:
```
TI-vreg = { git = "JITx-Inc/TI-VREG", version = "0.3.2" }
```

# TPS6208x
## Component
This is the family of Texas instruments [TPS6280](https://www.ti.com/lit/ds/symlink/tps62080.pdf) component.
```
inst buck : TI-vreg/components/TPS6208x/component(TI-vreg/components/TPS6208x/TPS62082DSG)
```
### Parameters
- pn:TPS6208x-PN - detailed part number for the family. Options:
```
public pcb-enum TI-vreg/components/TPS6208x/TPS6208x-PN:
  TPS62080DSG
  TPS62081DSG
  TPS62082DSG
  TPS62080ADSG
```

## Application circuit
This is a parametric application circuit for the TPS6280. 
Circuit Includes:
1.  Input capacitors
2.  Output filter
3.  Feedback network
```
  import power-systems

  ...


  val cxt-3v3 = power-systems/DC-DC/buck/BuckConstraints(
    v-in = min-max(4.9, 5.1)
    v-out = 3.3 +/- (3 %)
    v-in-ripple-max = 0.050
    v-out-ripple-max = 0.030
    i-out = 1.0 +/- (20 %)
    freq = 1.2e6
    K = (40 %)
  )

  inst DCDC-3v3 : TI-vreg/components/TPS6208x/circuit(
    TI-vreg/components/TPS6208x/TPS62082DSG
    cxt-3v3
    snooze-mode = false
    snooze-conn? = create-resistor(
        resistance = 0.0,
        precision = (1 %)
      )
    C-query? = cap-query
    )
```
### Ports
```
  port conv : power-converter
  port enable
  port power-good
```
### Parameters
- pn:TPS6208x-PN
- cxt:BuckConstraints,
- buck-sol?:Maybe<BuckSolution> = One(MFR-Suggested-Solution()),
  --
- snooze-mode:True|False
- snooze-conn?:Instantiable = ?
- C-query?:CapacitorQuery = ?
