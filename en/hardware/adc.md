# ADC

The processor has 8 ADC (Analog to Digital) converter inputs. The signals are 1.8V only interfaces. One of these, `AD7`, is connected to the PMIC (Power Management Integrated Circuit) TPS65217B and used for measuring voltages and current via the TPS65217B.

### ADC Inputs
The primary purpose of the ADC pins was intended for use as a Touchscreen controller but can be used as a general purpose ADC. Each signal is a 12 bit successive approximation register (SAR) ADC. Sample rate is 100K samples per second. There is only one ADC in the processor and it can be connected to any of the 8 ADC pins.

### VDD_ADC Interface
The signal `VDD_ADC` is provided via the expansion header, but is **not a voltage rail that is to be used to power anything on an expansion board**. It is supplied from the 1.8V rail of the TPS65217B and is run through an inductor for noise isolation. It is there if need for external circuitry to have access to the VREF rail of the ADC or to add additional filtering via a capacitor if needed.

