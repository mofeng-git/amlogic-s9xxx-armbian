# Allwinner One-KVM DTB Audit

本次审计范围以 `build-armbian/armbian-files/common-files/etc/model_database.conf` 中当前受支持的 Allwinner 设备为准。

- 机型条目数：4
- 唯一 DTB 数：4
- 实际二进制改动：3
- 原本已满足 `peripheral`：1

处理原则：

- 4 个受支持 DTB 都有明确的 USB OTG 控制器节点 `/soc/usb@5100000`。
- 该节点均为 `MUSB` 结构，且 `__symbols__` 中有 `usb2otg` 或 `usbotg` 别名可对应。
- One-KVM 目标统一为将 `/soc/usb@5100000` 的 `dr_mode` 调整为 `peripheral`。
- 其他 EHCI/OHCI/DWC3 host 节点保持原状，不做一刀切改动。

## Topology A: `/soc/usb@5100000`

适用结构：`MUSB OTG`

| DTB | 原始 `dr_mode` | 当前 `dr_mode` | 说明 |
| --- | --- | --- | --- |
| `sun50i-h6-tanix-tx6.dtb` | `host` | `peripheral` | MUSB OTG 节点改为 peripheral，USB3 host 保持 host |
| `sun50i-h6-tqc-a01.dtb` | `host` | `peripheral` | MUSB OTG 节点改为 peripheral，USB3 host 保持 host |
| `sun50i-h6-vplus-cloud.dtb` | `otg` | `peripheral` | MUSB OTG 节点改为 peripheral |
| `sun50i-h618-orangepi-zero3.dtb` | `peripheral` | `peripheral` | 已满足，无二进制改动 |
