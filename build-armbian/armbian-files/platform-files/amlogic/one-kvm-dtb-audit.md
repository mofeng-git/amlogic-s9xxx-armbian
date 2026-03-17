# Amlogic One-KVM DTB Audit

本次审计范围以 `build-armbian/armbian-files/common-files/etc/model_database.conf` 中当前受支持的 Amlogic 设备为准。

- 机型条目数：95
- 唯一 DTB 数：71
- 实际二进制改动：70
- 原本已满足 `peripheral`：1

处理原则：

- `usb-ctrl + dwc2 + dwc3` 结构：只改外层 OTG 控制器节点的 `dr_mode` 为 `peripheral`，保留内层 `dwc2` gadget 节点和 `dwc3` host 节点各自结构。
- `双 DWC2` 结构：只改已逐个确认的 OTG 路 `usb@c9000000` 为 `peripheral`，另一条 `usb@c9100000` 保持 `host`。
- 不按模糊命名猜测；审计基于每个 DTB 的实际 USB 节点结构、目标节点路径和原始 `dr_mode`。

## Topology A: `/soc/usb@ffe09000`

适用结构：`usb-ctrl + dwc2 + dwc3`

| DTB | 原始 `dr_mode` | 当前 `dr_mode` | 说明 |
| --- | --- | --- | --- |
| `meson-g12a-hg680-fj.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-g12a-s905l3a-cm311.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-g12a-s905l3a-e900v22c.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-g12a-s905l3a-m401a.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-g12a-sei510.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-g12a-x96-max-rmii.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-g12a-x96-max.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-g12b-a311d-khadas-vim3.dtb` | `peripheral` | `peripheral` | 已满足，无二进制改动 |
| `meson-g12b-a311d-oes-a.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-g12b-ali-ct2000.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-g12b-gtking-pro-h.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-g12b-gtking-pro-rev_a.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-g12b-gtking-pro.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-g12b-gtking.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-g12b-odroid-n2.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-g12b-s922x-oes-plus-b.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-g12b-ugoos-am6.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-a95xf3-air-gbit.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-a95xf3-air.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-h96-max-x3-oc.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-h96-max-x3.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-hk1box-vontar-x3-oc.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-hk1box-vontar-x3.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-skyworth-lb2004-a4091.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-tx3-bz-oc.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-tx3-bz.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-tx3-qz-oc.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-tx3-qz.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-ugoos-x3-oc.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-ugoos-x3.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-x88-pro-x3.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-x96-air-gbit.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-x96-air.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-x96-max-plus-100m.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-x96-max-plus-2101.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-x96-max-plus-a100.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-x96-max-plus-ip1001m.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-x96-max-plus-oc.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-x96-max-plus-q1.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-x96-max-plus-q2.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-sm1-x96-max-plus.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |

## Topology B: `/soc/usb@d0078080`

适用结构：`usb-ctrl + dwc2 + dwc3`

| DTB | 原始 `dr_mode` | 当前 `dr_mode` | 说明 |
| --- | --- | --- | --- |
| `meson-gxl-s905d-mecool-ki-pro.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxl-s905d-phicomm-n1-thresh.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxl-s905d-phicomm-n1.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxl-s905d-sml5442tw.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxl-s905l-venz-v10.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxl-s905l2-ipbs9505.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxl-s905l2-x7-5g.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxl-s905l3b-e900v22e.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxl-s905l3b-m302a.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxl-s905w-p281.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxl-s905w-tx3-mini.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxl-s905w-x96-mini.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxl-s905w-x96w.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxl-s905x-b860h.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxl-s905x-nexbox-a95x.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxl-s905x-p212.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxl-s905x-tbee.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxl-s905x-tx9.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxm-nexbox-a1.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxm-nexbox-a2.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxm-octopus-planet.dtb` | `otg` | `peripheral` | 手动确认缺少 `__symbols__.usb` 别名后，仍按外层 usb-ctrl 节点改为 peripheral |
| `meson-gxm-phicomm-t1.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxm-q201.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxm-t95z-plus.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxm-tx8-max.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxm-tx9-pro.dtb` | `otg` | `peripheral` | 外层 usb-ctrl 改为 peripheral |
| `meson-gxm-x92.dtb` | `host` | `peripheral` | 外层 usb-ctrl 改为 peripheral |

## Topology C: `/soc/usb@c9000000`

适用结构：`双 DWC2`

| DTB | 原始 `dr_mode` | 当前 `dr_mode` | 说明 |
| --- | --- | --- | --- |
| `meson-gxbb-beelink-mini-mx.dtb` | `host` | `peripheral` | `usb@c9000000` 改为 peripheral，`usb@c9100000` 保持 host |
| `meson-gxbb-mxq-pro-plus.dtb` | `host` | `peripheral` | `usb@c9000000` 改为 peripheral，`usb@c9100000` 保持 host |
| `meson-gxbb-p201.dtb` | `host` | `peripheral` | `usb@c9000000` 改为 peripheral，`usb@c9100000` 保持 host |
