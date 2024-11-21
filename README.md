# Installation

In slm.toml add:
```
TI-vreg = { git = "JITx-Inc/TI-VREG", version = "0.3.2" }
```

# TPS6208x
## Component
This is the family of Texas instruments [TPS6280](https://www.ti.com/lit/ds/symlink/tps62080.pdf) components. 
- Voltage input: 2.3 - 6.0V
- Voltage output: 0.5 - 4.0V
- Maximum current: 1.2A
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
- cxt:BuckConstraints
- buck-sol?:Maybe<BuckSolution> = One(MFR-Suggested-Solution())
  --
- snooze-mode:True|False
- snooze-conn?:Instantiable = ?
- C-query?:CapacitorQuery = ?

# TPS6293x
## Component
This is the family of Texas instruments [TPS6293x](https://www.ti.com/lit/ds/symlink/tps62932.pdf) components.
- Voltage input: 3.8 - 30.0V
- Voltage output: 0.8 - 22.0V
- Maximum current: 3.0A
```
inst buck : TI-vreg/components/TPS6293x/component(TI-vreg/components/TPS6293x/TPS62932DRL)
```
### Parameters
- pn:TPS6293x-PN - detailed part number for the family. Options:
```
public pcb-enum TI-vreg/components/TPS6293x/TPS6293x-PN:
  TPS62932DRL
  TPS62933DRL
  TPS62933ODRL
  TPS62933PDRL
  TPS62933FDRL
```

## Application circuit
This is a parametric application circuit for the TPS6293x. 
Circuit Includes:
1.  Input capacitors
2.  Output filter
3.  Feedback network
```
  val buck-5V-cst = power-systems/DC-DC/buck/BuckConstraints(
    v-in = min-max(10.0, 12.0),
    v-out = 5.0 +/- (2 %)
    v-in-ripple-max = 0.050
    v-out-ripple-max = 0.030
    i-out = 1.0 +/- (20 %)
    freq = 1.2e6
    K = (40 %)
  )

  inst DCDC-5V : TI-vreg/components/TPS6293x/circuit(
    TI-vreg/components/TPS6293x/TPS62932DRL,
    buck-5V-cst,
    freq = 1.2e6,
    SS-period = 10.0e-3 +/- 2.0e-3,
    UVLO = [15.0, 12.0]
  )
```
### Ports
```
  port conv : power-converter
```
### Parameters
- pn:TPS6293x-PN
- cxt:BuckConstraints,
--
- num-in-caps:Int = 2,
- num-out-caps:Int = 2,
- freq:Double|False = false,
- SS-period:Toleranced|False = false,
- UVLO:Double|[Double,Double]|False = false,
- buck-sol?:BuckSolution = ?
- C-query?:CapacitorQuery = ?
- R-query:ResistorQuery = ResistorQuery()

