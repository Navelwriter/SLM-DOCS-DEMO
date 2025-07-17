# Slot Mismatch

During startup or when updating the device's settings, the SLM-MX checks that the physical modules connected match what is expected. In simple terms:

- <b>Extra Modules:</b> If there are more modules connected than expected, the device will ignore the extras and work normally with the ones that are configured.
- <b>Missing or Incorrect Modules:</b> If there are fewer modules connected, or if the connected modules are not the correct ones (different type or in the wrong order), the device will enter a safe mode. This means it will turn off all outputs and show a specific LED signal to alert you — see [LED status page](led-status.md).

Once you update the settings so that they match the modules actually connected, the device will return to normal operation.

# Example
The MX has already been configured with 4 modules.

## Initial Configuration
::::{grid}
:::{grid-item}
:outline:
:columns: 2

<div align="center"><b>SLM-MX</b></div>

:::
:::{grid-item}
:outline:
:columns: 2

<div align="center"><b>Module 1</b></div>

:::
:::{grid-item}
:outline:
:columns: 2

<div align="center"><b>Module 2</b></div>

:::
:::{grid-item}
:outline:
:columns: 2

<div align="center"><b>Module 3</b></div>

:::
:::{grid-item}
:outline:
:columns: 2

<div align="center"><b>Module 4</b></div>

::::
## Scenario 1: Adding a new module to the end of the SLM-MX

::::{grid}
:::{grid-item}
:outline:
:columns: 2

<div align="center"><b>SLM-MX</b></div>

:::
:::{grid-item}
:outline:
:columns: 2

<div align="center"><b>Module 1</b></div>

:::
:::{grid-item}
:outline:
:columns: 2

<div align="center"><b>Module 2</b></div>

:::
:::{grid-item}
:outline:
:columns: 2

<div align="center"><b>Module 3</b></div>

:::
:::{grid-item}
:outline:
:columns: 2

<div align="center"><b>Module 4</b></div>

:::
:::{grid-item}
:outline:
:columns: 2

<div align="center"><b>[NEW]</b></div>

::::
In this case, the SLM-MX will show <b>Operation OK</b> even though the new module is not configured, and modules 1–4 will operate normally.

```{warning}
The new module is not configured, so it will not operate. If you want it to operate, you need to [configure the module](#configure-modules) in the configurator.
```

## Scenario 2: Adding a new module to the middle of the SLM-MX

::::{grid}
:::{grid-item}
:outline:
:columns: 2

<div align="center"><b>SLM-MX</b></div>

:::
:::{grid-item}
:outline:
:columns: 2

<div align="center"><b>Module 1</b></div>

:::
:::{grid-item}
:outline:
:columns: 2

<div align="center"><b>Module 2</b></div>

:::
:::{grid-item}
:outline:
:columns: 2

<div align="center"><b>[NEW]</b></div>

:::
:::{grid-item}
:outline:
:columns: 2

<div align="center"><b>Module 3</b></div>

:::
:::{grid-item}
:outline:
:columns: 2

<div align="center"><b>Module 4</b></div>

::::
In this case, the SLM-MX will show [**Slot Mismatch**](#slot-mismatch-status) because the order of the modules does not match the order of the modules in the configurator.  
No modules will operate until either the previous order is restored or the device is reconfigured.
