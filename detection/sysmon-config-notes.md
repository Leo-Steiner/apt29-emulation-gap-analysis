# Sysmon Configuration Notes

**Sysmon version:** 15.22
**Base config:** SwiftOnSecurity sysmon-config (source version 74, 2021-07-08)
**Platform:** Windows Server 2022

---

## Changes made for this project

### Change 1: ProcessAccessConfig enabled

The original SwiftOnSecurity config had `<ProcessAccessConfig/>` commented out. This meant Sysmon would **not** log Event ID 10 (ProcessAccess) at all, regardless of any rules in the `ProcessAccess` block.

**Change:**

```xml
<!-- Original (commented out): -->
<!-- <ProcessAccessConfig/> -->

<!-- Modified (enabled): -->
<ProcessAccessConfig/>
