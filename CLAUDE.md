# nepi_scripts — Developer Reference

## Purpose

`nepi_scripts` is the distribution repository for sample NEPI automation scripts. These are user-facing example scripts compatible with the NEPI automation framework — designed to be loaded and executed by `scripts_mgr` in `nepi_engine`. The repository currently contains only a LICENSE and README; no sample scripts have been added yet.

## Architecture

```
nepi_scripts/
├── LICENSE
└── README.md
```

This submodule is a placeholder. It contains no code at this time.

## How It Works

When scripts are added, they will follow the automation script pattern defined in `nepi_engine/nepi_sdk/scripts/automation_script_template.py`. That template defines the expected structure:

```python
NEPI_BASE_NAMESPACE = "/nepi/device1/"

class ScriptClassName(object):
    def __init__(self):
        # Initialize ROS publishers, subscribers, services
        pass
    def cleanup_actions(self):
        # Shutdown cleanup
        pass

if __name__ == '__main__':
    rospy.init_node(script_name)
    node = ScriptClassName()
    rospy.on_shutdown(node.cleanup_actions)
    rospy.spin()
```

Scripts placed in `/mnt/nepi_storage/nepi_scripts/` on the NEPI device are discovered by `scripts_mgr`, which lists them, enables/disables them via the RUI, and manages their process lifecycle (launch, monitor, stop with a 10-second SIGTERM timeout).

## Build and Dependencies

No build artifacts. Scripts are Python files run directly by `scripts_mgr` as subprocesses. They depend on `rospy` and whatever NEPI API or SDK modules they import. No catkin package definition is present in this submodule.

## Known Constraints and Fragile Areas

**This submodule is currently empty.** The README describes its purpose ("Sample automation scripts for NEPI Engine AI and Automation Solutions") but no scripts are present. Any reference to specific scripts in this repository should be verified against the actual contents before relying on them.

**Scripts run as subprocesses with a 10-second stop timeout.** Scripts that do not respond to SIGTERM within 10 seconds are killed by `scripts_mgr`. Scripts must handle shutdown cleanly via `rospy.on_shutdown()`.

**No sandboxing.** Scripts launched by `scripts_mgr` run with the same permissions as the NEPI process. Scripts in this repository have full access to the device's ROS network and filesystem.

## Decision Log

- 2026-03 — CLAUDE.md created — Initial developer reference, Claude Code authoring pass. Submodule is a placeholder with no scripts present.
